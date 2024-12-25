<section
  id="multiple-pages"
  data-item="Multiple Pages"
>
  <h2>Multiple Pages</h2>

If you are anything like me, you like to use a separate Markdown document for each section in your tutorial. Personally, I want to create a new `<section>` in my HTML file for each document that I create in Markdown. I also like to use the `<aside>` element for additional information that readers can skip if they are in a hurry.

This means treating three issues:

1. Getting pandoc to merge multiple MD documents into the same HTML output file
2. Getting pandoc to wrap the HTML output from each MD document in a `<section></section>` element, even though regular Markdown does not have the concept of sections.
3. Creating `<aside></aside>` elements, hopefully in a similar way.

Merging multiple MD documents is the easiest, so you can start with that.

1. Create a new file in your `Workflow/` directory, and call it `another.md`
2. Give it a YAML block with its own values for `title` and `subtitle`, and some text of its own, as shown in File FAK below

```md
---
title: Another Document
subtitle: Who goes first?
new: new value for a new
...

another.md has a \$new\$ variable
# \$title\$
## \$subtitle\$
Pandoc can merge multiple markdown files to the same HTML page.
```
File FAK.

1. In your `package.json` file, change your `"pandoc"` script to use `*.md` instead of `input.md` as the Markdown source.

```bash-#w
<i>{
  "scripts": {
    "pandoc": "pandoc -o index.html --filter filter.js --template=template.html </i><b>*.md</b><i>"
  },
  "dependencies": {
    "pandoc-filter": "^2.2.0"
  }
}</i>
```

This will allow you to see how the variables in the YAML blocks are applied both in the "template" part of the output file, and in the "markdown" part.

4. Press your new keyboard shortcut for `npm run pandoc`. (In the future, I'll just write "run `pandoc`", and you'll know to use your keyboard shortcut.)
5. Look at the HTML that pandoc generates for you in the `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <title>First Steps Complete</title>
</head>
<body>
  <p>another.md has a new value for a new variable</p>
  <h1 id="title">First Steps Complete</h1>
  <h2 id="subtitle">Success!</h2>
  <p>Pandoc can merge multiple markdown files to the same HTML page.</p>
  <h1 id="title-1">First Steps Complete</h1>
  <h2 id="subtitle-1">Success!</h2>
  <p>Use pandoc to convert this markdown file to HTML.</p>
  <p><a href="">A link which does nothing</a></p>
</body>
</html>
```

Is this what you expected? Do you notice the following points:

* The value used for `$title$` in `<title>First Steps Complete</title>` _was taken from the `input.md`_ file.
* The converted contents of both `another.md` and `input.md` have been added to `index.html`.
* The `another.md` file was processed first.
* The value for the `new` variable from `another.md` has been correctly substituted.
* The values for both `$title$ ` and `$subtitle$` in `another.md` were _taken from the `input.md` file_
* The `input.md` file was processed second, and all its values are the same as they were before.

## The same $variables$ name cannot have two values

From this, you can see that when the same YAML key is used with different values, only _one_ of those values is used. Another values for the same key will be discarded.

In this case, on my computer, the values from the second file that was processed overwrote the values from the first Markdown file. In the official pandoc documentation, the reverse situation is described. Perhaps this is an issue with the JavaScript port of `pandoc-filter`.

In any case, the lesson to be learned is that data will be lost if you use the same variable names with different values in different source files.

## A wild card to select all Markdown files

The `*` asterisk in the argument that indicates the name of the source Markdown file is a _wild card_. The name `*.md` will match all files whose names have the `.md` extension.

The files were processed in alphabetical order. If you want them to be processed in a specific order, you can prefix their names with numbers. For example:

1. Rename `input.md` to `01-input.md`
2. Rename `another.md` to `02-another.md`

> Note that I have used zeroes to pad the numbers. Files will be sorted alphabetically, and _alphabetically_ `11` comes after `1` and _before_ `2`. By using zero-padding, I can be sure that the files whose names begin with `01`, `02` and `11` will be treated in the order I want them to.

3. Run `pandoc`
4. Look at the HTML that pandoc generates for you in the `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <title>Another Document</title>
</head>
<body>
  <h1 id="title">Another Document</h1>
  <h2 id="subtitle">Who goes first?</h2>
  <p>Use pandoc to convert this markdown file to HTML.</p>
  <p><a href="">A link which does nothing</a></p>
  <p>another.md has a new value for a variable</p>
  <h1 id="title-1">Another Document</h1>
  <h2 id="subtitle-1">Who goes first?</h2>
  <p>Pandoc can merge multiple markdown files to the same HTML page.</p>
</body>
</html>
```

This time the `01-input.md` file was processed first, and the variables from the YAML block in `02-another.md` overwrote the values from `01-input.md`.

It's clear that it would be safest to use a YAML block only in one Markdown file.

> Now, what's the best way to create distinct sections in the HTML file, corresponding to the content of the different source Markdown documents?

</section>
