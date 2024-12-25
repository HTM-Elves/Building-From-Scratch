<section
  id="intro"
  data-item="Introduction"
>
  <h2>Introduction</h2>
The goal of this tutorial is to create a workflow for publishing your own tutorials online. This workflow will allow you to:

1. Write the content of your tutorials in Markdown (MD) using Microsoft's free [Visual Studio Code](https://code.visualstudio.com/) (VS Code)
2. Use VS Code snippets to insert commonly-used customizable chunks of Markdown
3. Write your tutorial in separate sections, with one MD document containing each section
4. Convert your set of MD documents to a single HTML page [using pandoc](https://www.youtube.com/watch?v=_gz9Vj8TMCc)
5. Use a template for the HTML page which contains:
   * `$variables$` for the title, the publication date, the associated GitHub repository and other details
   * Links to CSS files
   * A link to a JavaScript file that will automatically generate a collapsible menu for all the sections
   * Links to CSS and JavaScript files created by [Prism](https://prismjs.com/), in order to display code listings with highlighted syntax
6. Update the HTML version of your tutorial, with a simple keyboard shortcut, after you make changes to the MD text
7. View the updated HTML version live in multiple browsers

## Why pandoc?

I use [pandoc](https://pandoc.org/) to convert files from Markdown to HTML. There are two important reasons for this:
* pandoc can convert multiple Markdown files into a single HTML file
* pandoc allows you to customize the conversion process

## Customization in JavaScript

The official documentation for pandoc assumes that you are familiar with programming languages such as Haskell, Lua or Python. The tutorials that I want to publish are about web development with the MERN stack. This means that the programming language I am most proficient in is JavaScript. The published tutorial uses JavaScript to make it user-friendly.

For this reason, I have chosen to use JavaScript in the tutorial-publishing workflow. I understand that you may be writing tutorials for JavaScript-free situations, so I do not assume that you are familiar with JavaScript. I will take time to explain how it works. I do assume, however, that you have experience with other programming languages, and with HTML and CSS.

This tutorial will be very meta: a tutorial about writing and publishing this very tutorial. Hold on tight! 
</section>