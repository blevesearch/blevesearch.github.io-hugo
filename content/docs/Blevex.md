+++
author = ["Silvan Jegen"]
date = "2016-07-11T19:25:38-04:00"
title = "Blevex"
[menu.docs]
weight = 1
parent = 'analysis'
+++

## The Blevex experimental repository

In the blevex repository you can find code with experimental status. The repository contains the following types of code.

* language specific analyzer components
* kvstore bindings
* language detection analyzer
* stemmers


### Language-specific Analyzers

The following language-specific analyzers are available in blevex.

* Danish
* German
* English
* Spanish
* Finnish
* French
* Hungarian
* Italian
* Japanese
* Dutch
* Norwegian
* Portuguese
* Romanian
* Russian
* Thai
* Turkish

In order to use them you just import the language analyzer component(s) you are interested in and use it in your CustomAnalyzer. In case you want to use the German normalize filter you have to do something like this at indexing time.

Go
```

import "github.com/blevesearch/blevex/lang/de"

err = indexmapping.AddCustomAnalyzer("myde",
      map[string]interface{}{
        "type":      custom_analyzer.Name,
        "tokenizer": unicode.Name,
        "token_filters": []string{
          lower_case_filter.Name,
          de.NormalizeName,
        },
      })

(error handling has been left out for brevity)

```

Bleve will return an error if you open an index for which you haven't loaded all the necessary analyzer components. So if you want to open an index for which you have used one of the blevex analyzer components you will have to import the package (just for the side effect of having the analyzer components registered that way). To open an index built with the CustomAnalyzer above you will have to import the bleve 'de' package like this.


Go
```
import _ "github.com/blevesearch/blevex/lang/de"

```

Afterwards you should be able to just open the index using bleve.Open().

