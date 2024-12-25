<section
  id="replace-with-value"
  data-item=" Using $variable$ value"
>
  <h2> Replacing a $variable$ with its value</h2>


Now that you know what data pandoc sends to the `action()` function, you can check for any Block where the content is `$title$`... or indeed any single word that has a `$` dollar sign at each end.

Specifically, you will be looking for this Block:
```js-#101
type:   "Str"
content: "$title$"
format: html
meta:   {
  "title": {
    "t": "MetaInlines",
    "c": [
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
  }
}
```

When the `action()` function detects that it has received the Block where `content` is `"$title$"`, it should return this part of the `meta` entry, to replace `"$title$"`:

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

Pandoc will use this meta value instead of the string `$title$` when it converts its AST format to HTML.

The simplest way to do this would be to:

1. Add three lines of code to the `action()` function in `Workflow/filter.js`, as shown in Listing FAH below:

```js-#5
function action({t: type, c: content}, format, meta) {
  log(arguments)
  
  if (content === "$title$") {
    return meta.title.c
  }
}
```
Listing FAH.

> Note that now that you have seen what arguments the `action()` function receives, you can access them directly, inside the round parentheses that follow the name of the function.

2. In the Terminal window that is open on your `Workflow` directory, run the same command that you just used (Up arrow + Enter):
```bash-#
pandoc -o index.html --filter filter.js input.md
```

3. Look at what has been written in `Workflow/index.html`.

```html
<h1 id="title">First Dots</h1>
<p>Use pandoc to convert this markdown file to HTML.</p>
<p><a href="">A link which does nothing</a></p>
```

Success! The `$title$` variable in the `<h1>` header element has been replaced by the value of `title` taken from the YAML block: `First Dots`.

</section>