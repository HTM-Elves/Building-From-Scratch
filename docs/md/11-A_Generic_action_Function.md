<section
id="generic-action"
data-item="A Generic action()"
>

Using this regular expression, you can edit your `action()` function so that it will:

* Look for any `content` that has the `$doubledollar$` pattern
* Create a variable call `key`, whose value will be the same string but without the `$` dollar signs
* Check if the `meta` entry contains an entry for this dollar-free string, and if so, return the value for its `.c` entry.

> Remember that the `meta` entry looks like this...
> ```js-#
> meta:   {
>   "title": {
>     "t": "MetaInlines",
>     "c": [
>       {
>         "t": "Str",
>         "c": "First"
>       },
>       {
>         "t": "Space"
>       },
>       {
>         "t": "Str",
>         "c": "Dots"
>       }
>     ]
>   }
> }
> ```
> ... so if the value of `key` is `title`, `meta[key].c` will be the same as `meta.title.c`.

1. Edit the beginning of your `filter.js` script, so that it matches File FAJ below

```js-#3
const pandoc = require("pandoc-filter")
const REGEX = /^\$([a-z_.-]+)\$$/

pandoc.stdio(action);

function action({t: type, c: content}, format, meta) {
  log(arguments)
  
  const match = REGEX.exec(content)
  if (match) { // content is like `$title$`
    const key = match[1] // key will be like `title`
    const swap = meta[key]
    if (swap) {
      return swap.c  // an array from `meta`
    } // else returns undefined
  }
}
```
File FAJ.

If there is no match, or if the matching key has no entry in the `meta` array, then your `action()` function will return `undefined`, which is another of JavaScript's ways of saying "nothing at all".

As the [official documentation](https://pandoc.org/filters.html) says:
> If \[the `action()` function\] returns nothing, the node is unchanged; if it returns an object, the node is replaced.

When `content` is `"$title$`, the object that is returned will be ...
```js-#
[
  {
    "t": "Str",
    "c": "First"
  },
  {
    "t": "Space"
  },
  {
    "t": "Str",
    "c": "Dots"
  }
]
```
... which contains the AST for "First Dots".

### Testing the generic `action()`

To test if this new generic `action()` function works correctly, you can:

1. Change the value for `title` in the YAML block
2. Add a new variable in the YAML block of your `input.md` file, and use it in the body of the file, as shown in Listing LAI below:

```md
<i>---
title: </i><b>First Steps Complete</b>
subtitle: Success!</b><i>
...

# \$title\$</i>
<b>## $subtitle$</b><i>
Use pandoc to convert this markdown file to HTML.
[A link which does nothing]()</i>
```

2. In the Terminal window that is open on your `Workflow` directory, run the same command that you just used (Up arrow + Enter):
```bash
pandoc -o index.html --filter filter.js input.md
```

3. Check what was written to your `index.html` file. It should be something like this:
   
```html
<h1 id="title">First Steps Complete</h1>
<h2 id="subtitle">Success!</h2>
<p>Use pandoc to convert this markdown file to HTML.</p>
<p><a href="">A link which does nothing</a></p>
```
Congratulations! Pandoc has used the value of both YAML variables to replace their dollar-wrapped names. You have successfully created your first JavaScript filter for pandoc!

> You've made pandoc work with one simple Markdown file. To create a multi-part tutorial, you'll need to discover how to make pandoc produce a single HTML file from multiple Markdown files, with a shared HTML template.
>
> But this tutorial is about _workflow_ as much as it is about writing tutorials. You're going to be performing the same actions repetitively, and the more you write, the more time you can save if you first automate these actions.  

</section>