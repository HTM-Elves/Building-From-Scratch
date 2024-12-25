<!-- Proof of concept -->
<section
  id="proof-of-concept"
  data-item="Proof Of Concept">
  <a href="proof-of-concept">
     <h2>Proof Of Concept</h2>
  </a>  
You can start by creating a very simple Markdown document, and seeing how to convert it to HTML with pandoc.

1. Create a new directory to contain your tutorial-writing project. In the following text, I'll assume that you called this project directory `Workflow`.
2. Create a new file in this directory, called `input.md`
3. Write some Markdown text in `input.md`. For example, this...
```md
# First Steps
Use pandoc to convert this markdown file to HTML.

[A link which does nothing]()
```
File FA0.
... will create a level 1 header, a line of text, and an anchor link which leads nowhere.

> It's good to work directly on the Command Line. Here's a single command that you can use in a Terminal window, which will do all this for you:
> ```bash-#
> mkdir -p Workflow && cd Workflow && cat > input.md <<EOF
> # First Steps
> Use pandoc to convert this markdown file to HTML.
> 
> [A link which does nothing]()
> EOF
> ```
Console CAA.

1. [Install pandoc](https://pandoc.org/installing.html) on your computer, and check that it is working.
2. In a Terminal window that is open on your `Workflow` directory, run this command:
```bash-#
pandoc -o index.html input.md
```
Console CAB.

* The `-o` flag indicates that the next argument will be the (relative path to) the file where pandoc will generate the converted HTML
* The final argument (`input.md`) tells pandoc which file to convert.

By default, pandoc will use the extensions (`.html` and `.md`, in this case) to choose which file format to convert to and from.

6. Press the Enter key to execute this command.

You should see a new file created at `Workflow/index.html`. Here's what its contents should look like:

```html
<h1 id="first-steps">First Steps</h1>
<p>Use pandoc to convert this markdown file to HTML.</p>
<p><a href="">A link which does nothing</a></p>
```
File FAA.

7. Open this file in your browser.
   
![First Steps](images/firstStep.webp)
Figure IAA.

It looks like a web page, with a header, some text and a link.

8. Out of interest, open the original `input.md` file in your browser. You should see raw text, with no styling for the header and the link.

![Markdown in browser](images/mdInBrowser.webp)
Figure IAB.

> The `index.html` file is not good HTML. The file does not have a `DOCTYPE` declaration, a `<head>` or a `<body>` element, and other basic boilerplate is missing too.
> 
> The next step will be to provide a full HTML template to contain the converted MD file.

</section>
