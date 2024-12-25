<section
id="html"
data-item="HTML in Markdown"
>
<h2>HTML in Markdown</h2>

Guess what? Markdown allows you to include HTML elements, and pandoc will pass them through the conversion process unchanged.

From the [pandoc documentation](https://pandoc.org/chunkedhtml-demo/8.14-raw-html.html):

> Markdown allows you to insert raw HTML ... anywhere in a document...
>
> The raw HTML is passed through unchanged...

I want each Markdown document to be a separate `<section>` in the final HTML. I want each section to follow the same template. 

```html-#
<section
  id="section-id"
  data-item="Title for Menu"
>
<h2>Title for Page</h2>

Some content

</section>
```

The `template.html` that I use automatically generates a menu at the left of the page, using data from the section's attributes. See: that's how this tutorial that you are reading now works. if the viewport is wide enough, the menu will always be visible, if not, it will slide out of the way and leave a hamburger icon so you can show it again any time you want. Clicking on any menu item will show you the associated section, as if it were a separate page.

> ## Pandoc's custom markdown
> Pandoc provides custom markdown that can produce a similar template.
> 
> Pandoc Markdown
> ```md
> ::: {.section #section-id data-item="Title for Menu"}
> ## Title for Page
> Some content
> :::
> ```
>
> HTML output
> ```html
> <section id="section-id" data-item="Title for Menu">
> <h2 id="title-for-page">Title for Page</h2>
> <p>Some content</p>
> </section>
> ```
>
> Notice, however, that pandoc has added an `id="title-for-page"` to the `<h2>` element. Each `id` attribute in a page must be unique, so the automatic creation of this `id` that you did not ask for may lead to subtle problems. Not only that, but if the `id` that you choose is identical to that which would have been generated for the `<h2>` header, then _the  `<h2>` header does not get an `id` of its own.
>
> There are other reasons for not using pandoc's custom markdown for sections, which I describe below.

> _"Now wait a minute!"_ I hear you say. "I thought the whole point of this tutorial was to work in Markdown, and avoid writing any HTML.
>
> Yes. You are right. I'm going to _use_ HTML, but I'm going to get VS Code to write it for me. That's what I'll show you next.
</section>