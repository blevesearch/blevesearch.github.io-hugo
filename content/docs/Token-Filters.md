+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Token Filters"
[menu.docs]
weight = 30
parent = 'analysis'

+++

Analyzers reference Token Filters by name. Use existing ones or create variants with `IndexMapping.AddCustomTokenFilter`:
```
var m *IndexMapping = index.Mapping()
err := m.AddCustomTokenFilter("color_stop_filter", map[string]interface{}{
    "type": stop_tokens_filter.Name,
    "tokens": []interface{}{
        "red",
        "green",
        "blue",
    },
})
if err != nil {
    log.Fatal(err)
}
```
creates a new Stop Token Filter named "color_stop_filter", which removes all "red", "green" or "blue" tokens. Once registered, this filter can be referenced by a custom Analyzer.

### Apostrophe

Configuration:

* `type`: `apostrophe_filter.Name`

The Apostrophe Token Filter removes all characters after an apostrophe.

### Camel Case

The Camel Case Filter splits a token written in camel case into the set of tokens comprising it.  For example, the token `camelCase` would produce `camel` and `Case`.

### CLD2

The CLD2 Token Filter will take the text from each token and pass it to the [Compact Language Detection 2](https://code.google.com/p/cld2/) library.  Each token is replaced with a new token corresponding to the ISO 639 language code detected.  Input text should already be converted to lower case.

### Compound Word Dictionary

The compound word dictionary filter lets you supply a dictionary of words that combine to form compound words and lets you index them individually.

### Edge n-gram

The edge n-gram token filter will compute n-grams just like the n-gram token filter, but all the computed n-grams are rooted at one side (either the front or the back).

### Elision

The elision filter identifies and removes articles prefixing a term and separated by an apostrophe.

For example, in French `l'avion` becomes `avion`.

The elision filter is configured with a reference to a token map containing the articles.

### Keyword Marker

The keyword marker filter will identify keywords and mark them as such.  Keywords are then ignored by any downstream stemmer.

The keyword marker filter is configured with a token map containing the keywords.

### Length

The length filter identifies tokens which are either too long or too short.  There are two parameters, the minimum token length and the maximum token length.  Tokens that are either too long or too short are removed from the token stream.

### Lowercase

The Lowercase Token Filter will examine each input token and map all Unicode letters to their lower case.

### n-gram

The n-gram token filter computes n-grams from each input token.  There are two parameters, the minimum and maximum n-gram length.

### Porter Stemmer

The porter stemmer filter applies the Porter Stemming Algorithm to the input tokens.

### Shingle

The Shingle filter computes multi-token shingles from the input token stream.  For example, the token stream `the quick brown fox` when configured with a shingle minimum and maximum length of 2 would produce the tokens `the quick`, `quick brown` and `brown fox`.

### Stemmer

The stemmer token filter takes input terms and applies a [stemming](http://en.wikipedia.org/wiki/Stemming) process to them.

This implementation uses [libstemmer](http://snowball.tartarus.org/).

The supported languages are:

* Danish
* Dutch
* English
* Finnish
* French
* German
* Hungarian
* Italian
* Norwegian
* Porter
* Portuguese
* Romanian
* Russian
* Spanish
* Swedish
* Turkish

### Stop Token

Configuration:

* `type`: `stop_tokens_filter.Name`
* `stop_token_map` (string): the name of the token map identifying tokens to remove.

The Stop Token Filter is configured with a map of tokens that should be removed from the token stream.

### Truncate Token

The truncate token filter truncates each input token to a maximum token length.

### Unicode Normalize

The Unicode normalization filter converts the input terms into the specified [Unicode Normalization Form](http://unicode.org/reports/tr15/).

The supported forms are:

* nfc
* nfd
* nfkc
* nfkd
