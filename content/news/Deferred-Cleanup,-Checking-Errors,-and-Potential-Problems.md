+++
author = ["Marty Schoch"]
date = "2015-09-28T10:40:38-04:00"
title = "Deferred Cleanup, Checking Errors, and Potential Problems"
+++

The *defer* statement in Go is frequently used to ensure that once a resource has been acquired, it will be properly cleaned up.  In its simplest form it works exactly as you expect.  But, as you move to more advanced usages there are some things to watch out for.  I'd like to share one that I recently ran into while writing Bleve test cases.

### Basics

The power of the *defer* statement in Go is that it lets you put the acquisition and cleanup of a resource side by side.  When you read code later, it's easy to see that the correct behavior is guaranteed.  Let's take a look at a simple example:

```
func main() {
	r, err := Open("a")
	if err != nil {
		log.Fatalf("error opening 'a'\n")
	}
	defer r.Close()
}

type Resource struct {
	name string
}

func Open(name string) (*Resource, error) {
	return &Resource{name}, nil
}

func (r *Resource) Close() error {
	log.Printf("closing %s\n", r.name)
	return nil
}
```
[Run this in the Go Playground](http://play.golang.org/p/fYB2alVcIw)

The main function shows some very common behavior.  Open a resource, check for an error, then *defer* Closing the resource.  When we run this, we get the expected behavior:

```
2009/11/10 23:00:00 closing a
```

### Opening another Resource, Reusing the Variable

Let's make the example a bit more complex.  Now, after closing the resource, we will open another one (with a different name).  Can we reuse the same variable?

```
func main() {
	r, err := Open("a")
	if err != nil {
		log.Fatalf("error opening 'a'\n")
	}
	defer r.Close()

	r, err = Open("b")
	if err != nil {
		log.Fatalf("error opening 'b'\n")
	}
	defer r.Close()
}
```
[Run this in the Go Playground](http://play.golang.org/p/Bal_y0nv4U)

When I was new to Go and first encountered this code I wasn't sure if this would work.  I knew that *defer* would not execute until the end of main, but would it close 'a' AND 'b'?  Or would it close 'b' twice?  If we run it we see:

```
2009/11/10 23:00:00 closing b
2009/11/10 23:00:00 closing a
```

So, it does work, this is the expected output.  Some of you may be wondering, but *why* does it work?  As the Go blog [Defer, Panic, and Recover](http://blog.golang.org/defer-panic-and-recover) explains, 

> "A deferred function's arguments are evaluated when the defer statement is evaluated."  

In our case, the method receiver 'r' for the Close() method behaves just like an argument.  So the 'r' is evaluated at the time the defer statement is evaluated, and *NOT* when the statement is executed.  In Bleve code we saw this pattern occur frequently in test cases.  We would acquire a resource, take some action, close the resource, check the state of things, then repeat the process.

### errcheck?

Last February I had the amazing opportunity to attend [GopherCon India](http://www.gophercon.in/).  One of the many things I learned while there was of a tool called [errcheck](https://github.com/kisielk/errcheck).  The idea of the tool is simple, it looks at your Go code and identifies places where you're not checking a returned error.  This seemed like such an obvious thing to do, so I ran it on the entire Bleve codebase.  We found several clear cases where an error was returned, and we would not propagate the error back to the caller.

But what happens if we run *errcheck* on the code we just wrote above?

```
github.com/mschoch/defertest/main.go:10:15	defer r.Close()
github.com/mschoch/defertest/main.go:16:15	defer r.Close()
```

The Close() method returns an error.  Although it's not used in this contrived example, I coded it that way on purpose as that is very common in the real world.  Does checking this error matter?  If closing the first resource results in an error, should you try to close the second one?  In the abstract these are interesting philosophical questions, but let's assume we do want to check the errors.  How would we do it?  My first attempt looked like this:

```
func main() {
	r, err := Open("a")
	if err != nil {
		log.Fatalf("error opening 'a'\n")
	}
	defer func() {
		err := r.Close()
		if err != nil {
			log.Fatal(err)
		}
	}()

	r, err = Open("b")
	if err != nil {
		log.Fatalf("error opening 'b'\n")
	}
	defer func() {
		err := r.Close()
		if err != nil {
			log.Fatal(err)
		}
	}()
}
```
[Run this in the Go Playground](http://play.golang.org/p/_MPUl6zWjF)

I added an anonymous function which invokes r.Close(), checks the error, and if its non-nil we exit the program through log.Fatal().  It seems like such a simple change, but we've introduced a severe bug into the code.  In our case we can see it clearly since we're printing out the name of the resource being closed:

```
2009/11/10 23:00:00 closing b
2009/11/10 23:00:00 closing b
```

Oops!  We're now closing the 'b' resource twice, and never closing the 'a' resource.  This is a severe problem, closing 'b' a second time may not be well defined behavior, and not closing 'a' may leak resources.

Where did we go wrong?  Remember the rule we were given was:

> "A deferred function's arguments are evaluated when the defer statement is evaluated."

And in our new code, the deferred function has no arguments, and no method receiver.  We're now relying the anonymous function referring to 'r'.  The Go spec says:

> "...they may refer to variables defined in a surrounding function. Those variables are then shared between the surrounding function and the function literal, and they survive as long as they are accessible."

So, now it's fairly clear what happened.  At the time the *defer* statement was evaluated, the arguments were evaluated, but there were none.  Then at the end of the function, the deferred function is executed, and *now* as the anonymous functions are executed, 'r' is evaluated, and in both cases it refers to 'b'.

### The Fix

Option 1, we can rewrite the *defer* statement to pass 'r' as an argument:

```
func main() {
	r, err := Open("a")
	if err != nil {
		log.Fatalf("error opening 'a'\n")
	}
	defer func(r *Resource) {
		err := r.Close()
		if err != nil {
			log.Fatal(err)
		}
	}(r)

	r, err = Open("b")
	if err != nil {
		log.Fatalf("error opening 'b'\n")
	}
	defer func(r *Resource) {
		err := r.Close()
		if err != nil {
			log.Fatal(err)
		}
	}(r)
}
```

This does give the correct output:

```
2009/11/10 23:00:00 closing b
2009/11/10 23:00:00 closing a
```

But, to be honest, it's rather verbose.  In many cases it may just be simpler to use a different variable name.  Option 2, use a new variable for the second resource:

```
func main() {
	r, err := Open("a")
	if err != nil {
		log.Fatalf("error opening 'a'\n")
	}
	defer func() {
		err := r.Close()
		if err != nil {
			log.Fatal(err)
		}
	}()

	r2, err := Open("b")
	if err != nil {
		log.Fatalf("error opening 'b'\n")
	}
	defer func() {
		err := r2.Close()
		if err != nil {
			log.Fatal(err)
		}
	}()
}
```

As I mentioned earlier, in Bleve this pattern only seems to arise in test cases.  I suspect in real code you'd be more likely to already be using more descriptive identifiers.

### Summary

To me the lesson here is that we had a simple pattern for deferred cleanup that worked.  I even explicitly took the time to verify that it cleaned up the correct resources when I reused variables.  Then I took the seemingly natural step to make sure we check all the returned errors.  But, I was a bit careless and overlooked that the deferred anonymous function can have significantly different behavior.  It wasn't until I observed problems that I went back to recheck my assumptions and track down the problem.