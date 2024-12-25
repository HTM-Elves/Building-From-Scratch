<!-- 'Using Snippets -->
<section
  id="using-snippets"
  data-item="Using Snippets">
  <h2>Using VS Code Snippets</h2>

I don't want to type custom HTML in my Markdown document. I specifically chose to use Markdown so that I would have less to type.

VS Code provides a powerful set of code completion tools called [snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets). As you are typing in a document, VS Code will propose auto-completion suggestions. If you choose one by pressing the `Tab` or `Enter` key, VS Code will insert a chunk of predefined text, and allow you to customize it in predefined places.

Here's a simple snippet that I use to create the `section` template that I showed above.

```json-#
	"section": {
		"prefix": "section",
		"body": [
			"<section\n  id=\"${1:section-id}\"\n  data-item=\"${2:Title for Menu}\"\n>\n  <h2>${3:$2}</h2>\n  $0\n</section>"
		],
		"description": "Customize id, data-item and header. Add content."
	}
```

When I start typing the `prefix` (`section` in this case), VS Code opens a pop-up which allows me to select this snippet.

![Section Snippet](images/sectionSnippet.webp)

Notice that I only had to type the first two letters to make this appear. When I press the `Tab` key, this whole chunk of HTML is inserted in my Markdown page.

![Tab Stops](images/tabStops.webp)
```html-#
<section
  id="section-id"
  data-item="Title for Menu"
>
  <h2>Title for Menu</h2>
  
</section>
```

Even better! If you look closely at the image above you will see that the text `section-id` is selected. That's because this snippet contains several _tabstops_: editable zones where I can type the text I want instead of the placeholders.

* `${1:section-id}`
* `${2:Title for Menu}`
* `${3:$2}`

Better yet, the tabstops for the `data-item` and the `<h2>` text are linked.

When I press the `Tab` key, the selection jumps to the next tab stop. When I change the text for the `data-item` (the text which will appear in the menu), the text for the `<h2>` element changes at the same time.

![Multiple Cursors](images/MultipleCursors.webp)

I can then press the `Tab` key again, and tweak the the text for the `<h2>` element, if I want it to be more detailed than the entry in the menu.

> I imagine that you want to try this out for yourself. However, typing `section` into a Markdown document is unlikely to show any pop-up selector. You'll have to:
> * Activate `quickSuggestions` for Markdown in VS Code
> * Add a JSON block, like the one above, to your `markdown.json` settings file.


</section>