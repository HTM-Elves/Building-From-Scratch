<section
  id="yaml"
  data-item="Variables with YAML"
>
  <h2>Variables with YAML</h2>

When YAML was first created, the name was originally an acronym for "Yet Another Markup Language". Over time, it started being used more for data serialization than for markup, so now, officially, the name stands for "YAML Ain't Markup Language".

> The inspiration for YAML comes from Python, where whitespace at the beginning of a line has a meaning. For this reason, you should be careful not to leave white spaces at the beginning of a line, unless you know what you are doing. In this tutorial, I'll keep all text in YAML blocks left-aligned.

Here's how you can edit your `input.md` file to tell pandoc what value you want to use in place of the `$title$` variable in `template.html`:

```md
<b>---
title: First Dots
...</b><i>

# First Steps
Use pandoc to convert this markdown file to HTML.

[A link which does nothing]()</i>
```
File FAD.

The three `---` dashes at the start say: "YAML block starts here". The three `...` at the end say: "That's the end of the YAML block".

Inside the YAML block, you write a variable name (without enclosing `$` signs), followed by a `:` colon. The rest of the line represents the value of the variable.

In other words, this block...

```yaml
---
title: First Dots
...
```
File FAE.

... says "a variable called `title` will have the value 'First Dots'".

> I've used `First Dots` to remind you to end the block with dots.

1. Edit your `input.md` file so that it matches File FAD.
2. In the Terminal window that is open on your `Workflow` directory, run this command again):
```bash-#
pandoc -o index.html --template=template.html input.md
```
Console CAD.

> Tip: This is exactly the same as the last command you ran in the Terminal. When the Terminal window is active, you can press the Up arrow at the bottom right of your keyboard to step backwards through the history of your commands. Press the Enter key when you reach the command you want.
>
> You can also run the command `history` in your Terminal to see a numbered list of all your recent commands. The command `history X` (where `X` is the index number of an earlier command) will repeat that command.
>
> It's good to know shortcuts that allow you to do less typing.

3. Look at the `index.html` file. You should see that the `<title>` element now contains the value you defined in the YAML block.
   
```html-#
<i><!DOCTYPE html>
<html lang="en">
<head>
  <s><!-- Some lines skipped --></s>
  <title>First Dots</title>
</head>
<body></i><b>
  <h1 id="first-steps">First Steps</h1></b><i>
  <s><!-- More lines skipped --></s>
</body>
</html></i>
```
File FAD.

4. Refresh the file in your browser. You should see that the browser tab has the title that you chose.

![Title set](images/title.webp)
Figure IAC.

> Perhaps you want the text of the `<h1>` element to match the `<title>`? You might think that pandoc is smart enough to allow you to use the `$title$` variable inside the `input.md` file itself.
>
> SPOILER: pandoc isn't that smart. At least, not in that way.

</section>
