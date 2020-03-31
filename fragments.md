---
layout: default
---

# Fragments

Fragments are composable UI blocks in composition through Server Side Inclusion (SSI) using Edge Side Includes ([ESI](https://www.w3.org/TR/esi-lang)) or Client Side Inclusion (CSI) using [h-include](https://github.com/gustafnk/h-include).

A fragment consists of three parts: the markup, styles and behavior (js). We use the following convention:

- fragment.css.html
- fragment.js.html
- fragment.html

To include using ESI, on inclusion per instance:

```html
  <esi:include src="fragment.html"></esi:include>
```

The other two parts is included per "type" and could be placed in the document as you see fit:

```html
  <esi:include src="fragment.css.html"></esi:include>
```

```html
  <esi:include src="fragment.js.html"></esi:include>
```

Using ".html" lets the producer control versioning and cache busting.

## A tale of two folds

ESI limits you to 64 inclusions. But when optimizing your page, you might want to mix'n'match strategies. For example using SSI above the fold and CSI below the fold. [h-include](https://github.com/gustafnk/h-include) lets you compose lazily and optimize below the fold.

## Baseline(s)

There might be use for a thin layer of common dependencies like css reset, fonts etc. 