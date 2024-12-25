<section
id="regex"
data-item="Regular Expressions"
>
<h2>Regular Expressions</h2>


The version of the `action()` function that you have just written only works for one variable (`$title$`). To make it work for _any_ single word with a `$` sign at each end, you need to some changes.

A good way to check if a given string follows a specific pattern is to use a [Regular Expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions). A regular expression is a compact way of defining the rules for a pattern of characters.

Here is a Regular Expression which will detect if a string:

* Starts with a `$` sign
* Continues with a string of lowercase unaccented Latin characters (anything from "a" to "z")
* Ends with a `$` sign
* Contains no other characters except perhaps a `.` dot, an `_` underscore or a `-` dash.

```regexp-#
/^\$([a-z_.-]+)\$$/
```

This corresponds to the official [pandoc naming system for variables](https://pandoc.org/chunkedhtml-demo/6.1-template-syntax.html#interpolated-variables).

A regular expression look quite alien when you first see it, but there is method in this apparent madness.

>You can see how this regular expression works in [regex101.com](https://regex101.com/r/FhdRcu/2). The first five example strings match the pattern defined by the regular expression. The text on the right explains step-by-step how the regular expression works.
>
> Note how the `$` has two different meanings. When it is escaped with a backslash `\$` it means "dollar sign". Without the backslash, it means "final character". So `/\$$` means "a dollar sign followed by no other characters".

![RegEx101.com](images/regex.webp)

You'll notice that not only does the regular expression check if it matches the pattern, but it also extracts the characters between the `$` dollar signs, which regex101.com calls `Group 1`.

### Testing in the Node IDE

You can check how NodeJS treats regular expressions in an _integrated development environment_ (IDE) in your Terminal.

1. Open a new Terminal window
2. Run the command `node` to open the Node IDE
3. Create a variable `r` with the value of the regular expression you want to test:
```bash-#
r = /^\$([a-z_.-]+)\$$/
```
4. Evaluate the expression `r.exec("$title$")`

Your Terminal window should look something like this:

```bash-#
$ node
Welcome to Node.js v23.1.0.
Type ".help" for more information.
> <b>r = /^\$([a-z_.-]+)\$$/</b>
/^\$([a-z_.-]+)\$$/
> <b>r.exec("$title$")</b>
[ '$title$', 'title', index: 0, input: '$title$', groups: undefined ]
```

The expression `r.exec("$title$")` evaluates to an array. JavaScript starts counting from 0, so:

* Item `0` of the array is the full string that matches the regular expression.
* Item `1` gives the part of that match that is inside the _capturing group_ that is defined by the `()` round parentheses.
* The `index` value says that this match started when there were `0` characters to the left
* The `input` value reminds you of the entire string that you tested in order to find the given pattern
* `groups` has the value `undefined`, because this regular expression did not used an advanced feature where you can create capturing groups with specific names.

5. Now check what happens if the string inside the round parentheses does not match the pattern, or if it is not a string:
   
```bash-#
> <b>r.exec("$title")</b>
null
> <b>r.exec()</b>
null
> <b>r.exec(42)</b>
null
> <b>r.exec([])</b>
null
```

When there is no match, the expression evaluates to `null`, which is one of the ways JavaScript has for saying "nothing at all".

The important point to note is that when the string _does_ match the regular expression pattern, you get an array where the item `1` is the input string stripped of its `$` characters.

You can use the item `1` string to look for a replacement in the `meta` block.

</section>