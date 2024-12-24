<section
id="personal-content"
aria-labelledby="personal-content"
data-item="Personal Content"
>
<h2><a href="#personal-content">Personal Content</a></h2>

The content for this tutorial is stored in Markdown files inside the folder `docs/md/`. Edit these files and add to them to structure your tutorial.

To convert these files to a single HTML file at `doc/index.html`, you need to run the command `npm run pandoc`. This was already done for you after you ran the command `npm run new` which created this tutorial.

<details
class="note"
open
>
<summary>Pandoc</summary>
[Pandoc](https://pandoc.org/) is a free-software document converter, which can convert documents between around 60 different file formats. The languages used to write and extend it are Haskel, Lua and Python. A NodeJS module called `pandoc` allows you to access its power through JavaScript.

</details>

## Watching for changes to the `docs/md/` files

Before you start editing or adding to the files in `docs/md/`, you can run the command `npm run watch` in a Terminal window open at the root of this directory. This will start the `fb-watchman` node module, which will run `npm run pandoc` automatically for you:

* Whenever you save a modified `.md` file
* Whenever you add a new `.md` file to `docs/md/`
* Whenever you delete or rename an existing `.md` file.

<details
class="env"
open
>
<summary>Viewing with Live Server</summary>
If you are working with Virtual Studio Code, you can install the extension [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer), and use this to serve the page at `docs/index.html`. This will allow you to view the actual HTML output from your Markdown files in real time as you edit.

</details>
</section>
