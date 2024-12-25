<section
id="script-shortcuts"
data-item="Scripts and Shortcuts"
>
<h2>Scripts and Shortcuts</h2>

How many times have you already executed one of these commands in the Terminal?
```bash-#
pandoc -o index.html --template=template.html input.md
```
```bash-#
pandoc -o index.html --filter filter.js input.md
```

It's good that you can press the Up arrow key and then Enter, but you do have to select the Terminal window first. And if you have to do other work in the Terminal, there might be other commands that fill up your recent history.

Wouldn't it be good if you could just use a simple keyboard shortcut, while you are typing in the Text Editor?

Actually, soon you will be combining the two commands, and later you will need to add more arguments. I'll show you two techniques that you can combine to make your life simpler:

1. Creating an `npm` script
2. Creating a VS Code keybinding to run the script

The keybinding requires a trick that is not very intuitive, so I'll also show you a technique for:

3. Making keybindings for other commands that you might execute in the Terminal often.

## Creating an `npm` script

When you ran `npm install pandoc-filter` back in Section 6, `npm` created a file called package.json at the root of your `Workflow/` directory. Its contents currently look something like this:

```json-#
{
  "dependencies": {
    "pandoc-filter": "^2.2.0"
  }
}
```
1. Add a new "scripts" section, with a key called `"pandoc"` and a value which merges the two commands that you have already used, as shown below.

```json-#w
<i>{</i>
  <b>"scripts": {
    "pandoc": "pandoc -o index.html --filter filter.js --template=template.html input.md"
  },</b><i> 
  "dependencies": {
    "pandoc-filter": "^2.2.0"
  }
}</i>
```

2. Run this simpler command in the Terminal:
```bash-#
npm run pandoc
```

This tells the Node Package Manager to read the `"pandoc"` script that you just added to the `"scripts"` section of `package.json`, and to tell the active Terminal to run it. The most recent lines in your Terminal should look something like this:

```bash-#w
$ <b>npm run pandoc</b>

> pandoc
> pandoc -o index.html --filter filter.js --template=template.html input.md
```

> But this hasn't helped very much yet. You still have to activate the Terminal, and type something into it. In the next step, you'll see how to send use a keyboard shortcut to give this same command to the Terminal.

</section>
