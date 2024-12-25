<section
  id="html-template"
  data-item="An HTML Template"
>
  <h2>An HTML Template</h2>


Here's a basic template that you can use:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <title>$title$</title>
</head>
<body>
  $body$
</body>
</html>
```
File FAB.

It contains two variables that pandoc will recognize: `$title$` and `$body$`. When you give pandoc the right command, it will replace the `$body$` variable with the HTML that it created during the conversion process. For now, no value is defined for the `$title$` variable. You'll see how to work with that in the next step.

1. In your `Workflow/` directory, create a new file called `template.html`
2. Paste the template text from Listing AAA into it
3. In a Terminal window that is open on your `Workflow` directory, run this command:
```bash-#
pandoc -o index.html --template=template.html input.md
```
Console CAC.

Here, you have added the `--template=template.html` directive, which tells pandoc what file to use as the HTML template for the output.

4. Look at `Workflow/index.html`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >
  <title></title>
</head>
<body>
  <h1 id="first-steps">First Steps</h1>
  <p>Use pandoc to convert this markdown file to HTML.</p>
  <p><a href="">A link which does nothing</a></p>
</body>
</html>
```
File FAC.

You should see that the `$body$` variable has been replaced by the out put that you saw in the previous section. However, the `<title></title>` is empty. The `$title$` variable has been replaced by an empty string.

> In the next step, you will see how to create a [YAML](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started) block in your `input.md` file that will tell pandoc what value to use for `$title$`, and any other variables that you might choose to use.
</section>