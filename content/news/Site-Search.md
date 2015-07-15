+++
author = ["Marty Schoch"]
date = "2015-07-14T10:25:38-04:00"
title = "Site Search is Back!"
+++

We recently re-launched the Bleve website using Hugo, and we temporarily lost the ability to search the site.  Today, we bring back search, and show you how you can add search to your Hugo site.

At a high level, there are three steps to adding search to your site.  First, you must build the index.  Second, you must host the index.  Third, you add a search page to your site.

### Building the Index

1.  Install **hugoidx** - this is the command we will use build the search index.  Anytime you update your content and regenerate your site using the `hugo` command, you'll also want to rebuild your search index.

        go get github.com/blevesearch/hugoidx

2.  `cd <your hugo site>`
3.  `hugoidx`

	You should now have a file named `search.bleve`.

### Hosting the Index

In order to host the index we need to run a small Go program that is available on the internet.  To simplify this process, we have built a reusable application called `bleve-hosted`.  You can use this application safely answer queries to the index (read-only operations).

1.  Install `bleve-hosted`

		go get github.com/blevesearch/bleve-hosted

2.  `cd $GOPATH/src/github.com/blevesearch/bleve-hosted`
3.  `bleve-hosted`
4.  Test that its working:

		curl http://localhost:8080/api/test.bleve/_search -d '{"query":{"query":"bleve"}}'

	The resulting JSON should include "total_hits": 1

5.  Copy the `search.bleve` index you generated earlier into your `indexes/` folder.  (This can really be anywhere, it will always look for an `indexes/` folder relative to the current working directly when you launch `bleve-hosted`.)

6.  Restart `bleve-hosted` and optionally configure your server to keep this process running long term (init-scripts, etc)

### Add Search to your Site

Finally, we're ready to add a search page to our site.  Several files were downloaded as a part of the `hugoidx` package to help you get started.  Feel free to customize these files to best adapt them to your site.

1.  `cd <your hugo site>`
2.  Copy the main search page:

		cp $GOPATH/src/github.com/blevesearch/hugoidx/search.md content/

3.  Copy support JavaScript files:

		mkdir -p static/js/
		cp $GOPATH/src/github.com/blevesearch/hugoidx/handlebars.js static/js/
		cp $GOPATH/src/github.com/blevesearch/hugoidx/search.js static/js/

	handlebars.js is used to render search results using a simple template syntax.  
	search.js is our custom code to bind everything together.

4.  If your site is not already using jQuery:

		cp $GOPATH/src/github.com/blevesearch/hugoidx/jquery.min.js static/js/

	jQuery is used to make AJAX requests from the browser to `bleve-hosted`.

4.  Update your layout to include these javascript files.  For many sites this will be in a file like `layouts/partial/footer.html` or `themes/<your theme>/layouts/partials/footer.html`.  In the section where javascript files are being included you'll want to add:

		<script src="/js/jquery.min.js"></script>
		<script src="/js/handlebars.js"></script>
		<script src="/js/search.js"></script>

5.  Finally, we need to update search.js to point to the correct URL for `bleve-hosted`.  On line 2 of `static/js/search.js` modify the value:

		var searchURL = 'http://<your server>:8080/api/search.bleve/_search'

### Try it Out

Now, you're ready to regenerate your site and try it out.  If you open your browser to:

		http://localhost:1313/search

You should see the standard search box.  If you type in your query, the page should reload and display search results below.  If you run into problems, it may be helpful to view the javascript console.

### Finishing Touches

Now let's also add search to the navigation bar.  For our site, we modified the partial `navbar.html` to include the following inside the navigation unordered-list:

        <div class="dropdown pull-right">
          <form class="navbar-form" role="search" action="/search">
            <div class="input-group">
                <input type="text" class="form-control" placeholder="Search" name="q" id="srch-term">
                <div class="input-group-btn">
                    <button class="btn btn-default" type="submit"><i class="glyphicon glyphicon-search"></i></button>
                </div>
            </div>
          </form>
        </div>

### Future

Is this perfect? No, not really, there are a still a lot of rough edges we'd like to smooth out.  Here are some of our ideas for the future:

1.  Enable running `bleve-hosted` in Google App Engine.  This lowers the bar for hosting your index.
2.  Streamline the addition of search to your site.  The manual copying of multiple files and editing paths is error prone.  Perhaps additional sub-commands of `hugoidx` could assist with this.
3.  Document worklfow for keeping your site AND your search index up to date.

Having problems with these instructions?  Let us know via [email](mailto:info@blevesearch.com) or the [Google Group](https://groups.google.com/forum/#!forum/bleve).
