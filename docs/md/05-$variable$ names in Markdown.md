<section
  id="$variable$-fail"
  data-item="$variable$ fails in MD"
>
  <h2>`$variable$` names fail in Markdown</h2>

To test how pandoc treats a string like `$title$` when it is part of the body of the Markdown document...

1. Edit your `Workflow/input.md` file so that it matches File FAE below. (Replace "# First Steps" with `# $title$`.)
   
```md
<i>---
title: First Dots
...
  
# </i><b>$title$</b><i>
Use pandoc to convert this markdown file to HTML.

[A link which does nothing]()</i>
```
File FAE.

2. In the Terminal window that is open on your `Workflow` directory, run the same command again (Up arrow + Enter):
```bash-#
pandoc -o index.html --template=template.html input.md
```
Console CAE.

3. Look at what has happened in `Workflow/index.html`. The `h1` title has been treated to a lot of `<em>` tags and a `"math inline"` class. This is not what you wanted.

```html
<!DOCTYPE html>
<s><!-- Some lines skipped --></s>
  <h1 id="title"><span
  class="math inline"><em>t</em><em>i</em><em>t</em><em>l</em><em>e</em></span></h1>
<s><!-- More lines skipped --></s>
</html>
```

> The [official pandoc documentation](https://pandoc.org/chunkedhtml-demo/8.13-math.html) explains:
> 
> ---
> Anything between two $ characters will be treated as TeX math.  
> ...  
> If for some reason you need to enclose text in literal $ characters, backslash-escape them and they won’t be treated as math delimiters.

So what happens if you use `\$` instead of `$`?

4. Edit your `Workflow/input.md` file as shown in 
File FAF below, and see:
```md
<i>---
title: First Dots
...
  
# </i><b>\$</b><i>title</i><b>\$</b><i>
Use pandoc to convert this markdown file to HTML.

[A link which does nothing]()</i>
```
File FAF.

5. In the Terminal window that is open on your `Workflow` directory, run the same command again (Up arrow + Enter):
```bash-#
pandoc -o index.html --template=template.html input.md
```
Console CAF.

7. Look at what has happened in `Workflow/index.html`. There's no hint of `<em>` tags or class names now. The `h1` title now shows... the raw string `$title$`. Pandoc has not used the value of the `title` variable from the YAML block.

```html
<i><!DOCTYPE html>
<s><!-- Some lines skipped --></s></i>
  <b><h1 id="title">$title$</h1></b>
<i><s><!-- More lines skipped --></s>
</html></i>
```

Pandoc has simply understood that `\$title\$` should be treated as a string with escaped `$` dollar signs, but it hasn't recognized it as a variable name.

> The next step will be to create a _filter_ which will tell pandoc that you want any word between two `$` characters to be treated as a variable.

</section>
