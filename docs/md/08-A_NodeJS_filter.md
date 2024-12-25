<section
id="nodejs-filter"
data-item="A NodeJS Filter"
>
<h2>A NodeJS Filter</h2>

To look into the box where pandoc has created its AST version of the `input.md` file:

1. Create a new file called `filter.js`
2. Paste the text from Listing LAF into it.

I'll describe what the code does, step-by-step, below.

> It's not important how the `// LOGGING` section works though, so you can skip that explanation if your want. You often use tools that just work (like your smartphone or your microwave), without knowing exactly how they work. You can think of them as "black boxes" where all that matters is the input and the output, but not what happens inside. You can think of the logging process as just another black box.

```js
#!/usr/bin/env node

const pandoc = require("pandoc-filter")

pandoc.stdio(action)

function action() {
  log(arguments)
}



// LOGGING //

const { createWriteStream } = require('fs')
const logger = new console.Console(createWriteStream("filter.log"));

function log(args) {
  const [{t: type, c: content}, format, meta] = args
  logger.log(`type:   "${type}"`)
  if (content) {
    logger.log(`content: ${JSON.stringify(content, null, 2)}`)
  }
  logger.log(`format: ${format}`)
  logger.log(`meta:   ${JSON.stringify(meta, null, 2)}`)
  logger.log("\n-----\n")
}
```
Listing LAF.

3. In the Terminal window that is open on your `Workflow` directory, run a new command:
```bash-#
pandoc -o index.html --filter filter.js input.md 
```

Here, the `--filter` flag tells pandoc to show each "bead" that it has created in the AST format to the `fiter.js` script. The `filter.js` will run `log()` for each "bead". This will append some text to a file named `filter.log`, with 

> Note that this command does not include the `--template` directive, so the contents of `index.html` will not be a complete HTML document. Only the result of the conversion of `input.md` will appear.
>
> You can restore the `--template` directive later.

4. You should now find a new file at `Workflow/filter.log`. It starts like this.

```js
type:   "Str"
content: "First"
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
// Hundreds of lines skipped
```
File FAG.

5. Notice that:
   * Each "bead" has a blank line between it and the next. File FAG above shows just the first bead. The official name for a "bead" is a `Block`.
   * Each Block has entries for:
     - `type`
     - `format`
     - `meta`
   * Most Blocks also have an entry for `content`. (There's no `content` for a Block with `type: "Space"`)
   * For many Blocks, the `content` is a word in double quotation marks, like `"First"` or `"Dots"`.
   * For some Blocks, the `content` has a more complicated structure. (It is an _array_.)
   * **Every Block** has the same entry for `meta`.
  
## `meta`

This is what the `meta` entry looks like, for every Block.

```js-#
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

6. Compare this with the YAML block at the top of your `input.md` file:
   
```md
---
title: First Dots
...
```

Do you see what the `meta` entry says, in its own format?It says: "There's a variable named `title` and its value is: `"First"`, followed by a `Space`, and `"Dots"`. In simpler terms: `"First Dots"`.

7. Look at the Block that starts at line 101 or so:

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

This says: "There's a Block with the content `"$title$"`.

Your next task will be to replace the content `"$title$"` with the array from the `meta` entry.

---

## `filter.js`, line by line

I promised to explain what the code in `filter.js` does, so I'll do that now. Most of it is just temporary code to help you see what pandoc's AST Blocks look like, so you can skip ahead to the next section if you want.

```js
#!/usr/bin/env node
```

The very first line tells the program that wants to execute this file that it is designed to be use with `node`, which is the code name for NodeJS. You can find more details about this in this [StackOverflow answer](https://stackoverflow.com/a/33510581/1927589).


```js-#3
const pandoc = require("pandoc-filter")
```

This line tells `node` to use the `pandoc-filter` package. Using the food-mixer metaphor, it says: "Fix on the `pandoc-filter` attachement before pressing the Start button". It gives this package the code name `pandoc`, so that it can be told what to do, later.

```js-#5
pandoc.stdio(action)
```

This line says to the `pandoc-filter` package: "When you have some IO (input-output) work to do, tell the `action` function (see below) about what you are about to work on.

### The `action()` function

For every Block in the AST version of your `input.md` text, pandoc will send some data to the `action()` function. This is the equivalent of you looking at each bead on the necklace.

```js-#7
function action() {
  log(arguments)
}
```

For now, all that happens is that the data (`arguments`) that pandoc sends will be treated by the `log()` function (see below).

For now, the `action()` function does not change anything in the data. It just looks at it, notes it into the `filter.log` file, nods its head and says: "Next?"

In short, the `action()` function intercepts data after pandoc has convented it to its intermediate AST format, and before it has started converting it to HTML.

### Logging

In order to show you what each AST Block looks like, I have created a `logger` object and a `log` function.

> The section of the script that starts with `// LOGGING` is only temporary. It just helps you to understand what pandoc is doing. Later, when everything is working the way you want it, you will be able to delete this section of the script.

```js-#15
const { createWriteStream } = require('fs')
const logger = new console.Console(createWriteStream("filter.log"));
```

The `logger` uses a built-in module for NodeJS called `fs`, which is short for `File System`. This allows me to write data to the file `filter.log`. Because I have given just the name of the file the `fs` module will automatically create this in the same directory as the `filter.js` script itself: `Workflow/`.

```js-#18
function log(args) {
  const [{t: type, c: content}, format, meta] = args
  logger.log(`type:   "${type}"`)
  if (content) {
    logger.log(`content: ${JSON.stringify(content, null, 2)}`)
  }
  logger.log(`format: ${format}`)
  logger.log(`meta:   ${JSON.stringify(meta, null, 2)}`)
  logger.log("")
}
```

I know, because I have looked at the source code for the `pandoc-filter` module, that the `action()` function will receive three _arguments_ (that is, three separate pieces of data.) Specifically, line 91 (or so) in the file at `node_modules/pandoc-filter/index.js` will call:

```js-#91
var res = action(item, format, meta)
```

By looking at the values of `item`, `format` and `meta` before I wrote this script, I was able to see that `item` is an object with two parts, with the _keys_ `"t"` (for `tag` or `type`) and `"c"` (for `content`) similar to what you have seen in the `filter.log` file. For example:

```js-#
{
  "t": "Str",
  "c": "First"
}
```
or
```js-#
{
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
```
I thought it would be more human-readable to convert `t` to `type` and `c` to content. So the line...

```js
const [{t: type, c: content}, format, meta] = args
```

...allows me to give a name to all the individual chunks of data that I want to know about. This process is called _destructuring_ and you can read more about it [on the MDN website](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

### `logger.log(...)`

The `logger.log(...)` statements append data to the `filter.log` file.

```js-#
JSON.stringify(<data>, null, 2)
```

Some of the data is actually a hierarchy of data. The [`JSON.stringify()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) statements ensure that all the data is shown, with one item per line, indented by `2` spaces for each hierarchical level.

Blocks with `type: "Str"` do not have an entry for `content`, so there is an `if (content) { ... }` block which only logs a content entry if such an entry exists. 
</section>