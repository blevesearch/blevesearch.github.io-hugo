+++
author = ["Marty Schoch"]
date = "2015-06-23T10:25:38-04:00"
title = "Query String Query"
[menu.docs]
weight = 15
parent = 'queries'

+++

The query language query allows humans to describe complex queries using a simple syntax.

### Terms
Plain terms without any other syntax are interpreted as a match query for the term in the default field.  The default field is `_all` unless overridden in the index mapping.

Example: `water` will perform a [Match Query]({{< relref "docs/Query.md#match" >}}) for the term `water`.

### Phrases
Phrase queries can be accomplished by placing the phrase in quotes.

Example: `"light beer"` will peform a [Match Phrase Query]({{< relref "docs/Query.md#match-phrase" >}}) for the phrase `light beer`.

### Field Scoping
You can qualify the field for these searches by prefixing them with the name of the field separated by a colon.

Example: `description:water` will perform a [Match Query]({{< relref "docs/Query.md#match" >}}) for the term `water`, in the `description` field.

### Required, Optional, and Exclusion
When your query string includes multiple items, by default these are placed into the SHOULD clause of a [Boolean Query]({{< relref "docs/Query.md#boolean" >}}).

You can change this by prefixing your items with a `+` or '-'.
* '+' Prefixing with plus places that item in the MUST portion of the boolean query.
* '-' Prefixing with a minus places that item in the MUST NOT portion of the boolean query.

Example: `+description:water -light beer` will perform a [Boolean Query]({{< relref "docs/Query.md#boolean" >}}) that MUST satisfy the Match Query for the term `water` in the `description` field, MUST NOT satisfy the Match Query for the term `light` in the default field, and SHOULD satisfy the Match Query for the term `beer` in the default field.  Result documents satisfying the SHOULD clause will score higher than those that do not.

### Boosting
You can influence the relative importance of the clauses by suffixing clauses with the ^ operator followed by a number.

Example: `description:water name:water^5` will perform Match queries for `water` in both the `name` and `description` fields, but documents having the term in the `name` field will score higher.

### Numeric Ranges
You can perform numeric ranges by using the >, >=, <, and <= operators, followed by a numeric value.

Example: `abv:>10` will perform an [Numeric Range Query]({{< relref "docs/Query.md#numeric-range" >}}) on the `abv` field for values greater than ten.

### Date Ranges
You can perform date range searches by using the >, >=, <, and <= operators, followed by a date value in quotes.

Example: `created:>"2016-09-21"` will perform an [Date Range Query]({{< relref "docs/Query.md#date-range" >}}) on the `created` field for values after September 21, 2016.

### Prefix wildcard
You can perform prefix wildcard queries by adding `*` to the term. This is specially useful for typeahead systems.

Example: `lig*` will perform a [Prefix Query]({{< relref "docs/Query.md#prefix" >}}) matching items that start with `lig`, including       `light`, `lightning` and `ligature`.

You can also use Field Scoping, limiting the results to a specific field, for example: `name:lig*` will match records in where the `name` field starts with `lig`.

`*` will match any sequence of characters.

`?` will match a single character.

### Escaping

The following quoted string enumerates the characters which may be escaped:

```
"+-=&|><!(){}[]^\"~*?:\\/ "
```

NOTE: this list contains the space character.

In order to escape these characters, they are prefixed with the \ (backslash) character.  In all cases, using the escaped version produces the character itself and is not interpreted by the lexer.

Example: `my\ name` will be interpreted as a single argument to a match query with the value "my name".

Example: `"contains a\" character"` will be interpreted as a single argument to a phrase query with the value `contains a " character`.
