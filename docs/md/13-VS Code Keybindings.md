<section
id="keybindings"
data-item="VS Code Key Bindings"
>
<h2>VS Code Key Bindings</h2>

Virtual Studio Code allows you to customize your keyboard shortcuts or [Key Bindings](https://code.visualstudio.com/docs/getstarted/keybindings).

It even allows you to create [Key Bindings for the Terminal](https://code.visualstudio.com/docs/terminal/advanced#_custom-sequence-keybindings). Specifically, this "only works with the \\u0000 format". Scary?

I'll show you first how to create a key binding that will run the command `npm run pandoc`, even if you don't have an integrated Terminal window open.

1. Open the Command Palette. The lazy way to do this is to select the menu item View > Command Palette.
    ![Command Palette](images/CommandPalette.webp)

   You'll notice that the menu item shows a keyboard shortcut: Ctrl-Shift-P. In the future, you can use the shortcut instead and save a few precious seconds.
   
> There's an even shorter shortcut though. With only one finger, you can press the `F1` key on your keyboard. However, your keyboard may be set up to activate a media control when you press `F1`. In that case, you _can_ press both the `fn` and the `F1` keys, but a better solution is to toggle the function lock. You can [seach on Google](https://www.google.com/search?q=fn+function+lock) to see how to do this on your operating system.

2. With the Command Palette open, start typing the word `keyboard`. You won't have to type the whole word before the auto-complete system will suggest `Preferences: Open Keyboard Shortcuts (JSON)`.

![Keyboard Shortcuts](images/keyboard.webp)

3. Press Enter to select this item (or lose a few precious seconds by moving your mouse to click and make the selection).
4. A new VS Code window will open, showing you the `keybinding.json` file where all your keyboard shorcuts are stored. Copy the following JSON object...

```json-#
,
{
    "key": "ctrl+shift+alt+p", // use your own shortcut
    "description": "Runs \'npm run pandoc'\ in the  Terminal",
    "command": "workbench.action.terminal.sendSequence",
    "args": {
        "text": "\u006E\u0070\u006D\u0020\u0072\u0075\u006E\u0020\u0070\u0061\u006E\u0064\u006F\u0063\u000A"
    }
}
```

5. ... and paste it at the end of the of the file, as shown in File Listing XXX above.

> Make sure that there is a final `}` closing curly brace _after_ the chunk of text that you pasted in, and that there are no extra `,` comma characters. If the `{}` curly braces or commas are not correctly balanced in this file, it could fail to work.

6. I've chosen the key binding `ctrl+shift+alt+p` (with `p` standing for `pandoc`), but you can edit the entry for `"key"` to suit your own preferences.

7. Bring to the front the window where you are working on your Workflow project and press the keyboard shortcut that you chose. If all goes well, you should see the following in your Terminal window:

```bash-#w
<b>npm run pandoc</b>

> pandoc
> pandoc -o index.html --filter filter.js --template=template.html input.md
```

In other words, with a single keyboard shortcut, you have been able to run the whole `pandoc -o index.html --filter filter.js --template=template.html input.md` command. All that matters is that the project window that contains your `package.json` file is the active window. It doesn't matter which panel in that window you are working in.

As you learn more about pandoc, you can update the `"pandoc"` script in the `package.json` file, and continue to use the same keyboard shortcut to execute it.

> This shortcut expects that:
> 
> * You have a file named `package.json` in your active project
> * Your `package.json` contains a `"scripts"` section where a script named `"pandoc"` is defined
> 
> If you press this keyboard shortcut in VS Code when working on a different project, you might see one of these errors:
> ```bash-#
> npm run pandoc
> npm error code ENOENT
> npm error syscall open
> <s># + more error details</s>
> npm error enoent Could not read package.json
> ```
> ```bash-#
> npm run pandoc
> npm error Missing script: "pandoc"
> <s># + more error details</s>
> ```
> This is normal. You would get exactly the same error if you explicitly typed `npm run pandoc` in a Terminal that is unable to make sense of this command.

> But what's this strange `"text":"\u006E\u0070\u006D\u0020\u0072\...` entry, and how does it work? That's what you will see next... if you want to.
</section>
