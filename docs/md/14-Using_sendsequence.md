<section
id="sendsequence"
data-item="Using .sendsequence"
>
<h2>Using `workbench.action.terminal.sendSequence`</h2>

When you create [key bindings for the Terminal](https://code.visualstudio.com/docs/terminal/advanced#_custom-sequence-keybindings), you have to talk in the  language that the Terminal understands.

When you look at this page, you see words. Since your early childhood, you have trained your brain to recognise that footprints in the mud combine to show where someone was going, and that letters on a page combine to give a specific meaning. You have built up a personal knowledge of the world that computers do not have.

All that computers can see are zeroes and ones. Actually, they don't even see that; they see two contrasting electro-magnetic patterns, that we humans like to call zeroes and ones.

So when you see the letter "a" on your screen, the computer sees the electro-magnetic equivalent of `1100001`, which is the binary representation of the number 97. You are used to reading Latin letters (A-Z, a-z), but there are hundreds of different writing systems, and some, like Chinese, have many more characters than the Latin alphabet. Over the centuries, humans have created thousands of meaningful characters.

Humans have developed a system called _Unicode_ which is designed to represent every character, symbol, emoji and meaninful squiggle into a number that a computer can store. It even has numbers for characters that you can't see, like [U+200B ZERO WIDTH SPACE](https://en.wikipedia.org/wiki/Zero-width_space) and  control characters like [U+000A LINE FEED](https://en.wikipedia.org/wiki/Unicode_control_characters#Category_%22Cc%22_control_codes_(C0_and_C1)), which says "the next character starts on the next line".

For you, the numbers `1` and `11` are very different. But if you put them side-by-side, you wouldn't know if your were looking at "one" and "eleven", or "eleven and one" or "one hundred and eleven". One easy solution is to say: "We will only count from 0 to 99, and any single-digit numbers must be padded with a zero." With this rule, it's easy to see the difference between `0111` ("one" and "eleven") and `1101` ("eleven and one")... but "one hundred and eleven" cannot be written in this system.

> You'll be padding numbers with zeroes yourself shortly.

The Unicode system uses thousands of different numbers, so it needs to use a fixed number of separate spaces, big enough to hold the biggest numbers, and it needs to pad all numbers except for the very biggest with zeroes.

One standard is to use 4 hexadecimal characters for each number, from `0000` (zero) to `FFFF` (65535). Each hexadecimal digit is a shorthand for 4 _bits_ (or binary digits). These range from `0000` (binary zero) to `1111` (8 + 4 + 2 + 1 = 15). This means that the system will use numbers that are stored with 16 bit spaces..

The letter "a" is represented by the number `97` which is `4 x 16 + 1` or `41` in hexadecimal, or `0041` in the Unicode system that I am describing.

To tell the computer to expect that a particular group of 16 zeroes-and-ones is actually a Unicode character, the number is preceded (in human eyes) by `\u`. So "a" for you is `\u0041` to the computer.

If you visit the [DenCode](https://dencode.com/en/string/unicode-escape) website, you can paste the sequence `\u006E\u0070\u006D\u0020\u0072\u0075\u006E\u0020\u0070\u0061\u006E\u0064\u006F\u0063\u000A` into the input field, and you will see `npm run pandoc⏎` in the output field.

Actually, you won't see the `⏎`LINE FEED character, because it's invisible, but it's there. It has to be included in the sequence that you send to the Terminal, so that the Terminal knows not just to display the string `npm run pandoc` but also to do the equivalent of pressing the Enter key, to execute the command.

## Converting to Unicode Escape format 

If you want to create your own key bindings to use with the 
`"workbench.action.terminal.sendSequence"` command, you can use the create a script that will do this for you.

1. Create new folder, in the place where you keep stuff that might be useful later
2. Create a new file in this new folder and call it `createKeyBinding.js`
  
> if you want to do this all from the Terminal, run :
> ```bash-#w
>  # Edit the following line to choose your own target directory:
> <b>dir=~/Documents/CoolTools/npm</b>
> # Create the directory, if necessary, and cd into it
> <b>mkdir -p $dir && cd $dir</b>
> # Create a new (empty) file and open it in VS Code
> <b>code createKeyBinding.js</b>
> ```

3. Paste the following code into `createKeyBinding.js`. Yes, it's long, but it's mostly explanatory comments.

```javascript
// Read input from the command line.

// `process.argv` is an array. The first two items will be
//
// 0: The absolute path to the node executable file
// 1: The absolute path to the file you want node to run
//
// Any remaining items will be the words that followed the
// `node <scriptname>` command. For example, if you run the
// command...
//
//    node createKeyBinding.js three word argument
// 
// ... process.argv will be something like this:
//
// [
//   '/path/to/node',
//   '/path/to/createKeyBinding.js',
//   'three',
//   'word',
//   'argument'
// ]

// console.log("process.argv:", process.argv, "\n\n")

// Ignore the first two items, and just keep the words that were
// given after the name of the file to run...
let input = process.argv.slice(2)

// ... or use "npm run pandoc" if no input is given
if (!input.length) {
  input = "npm run pandoc"

} else {
  // Input from the command line will be separate words that need
  // to be joined into one continuous string.
  input = input.join(" ")
}

// Convert input to an array of individual characters
const charArray = [...input] // or input.split("")

// Define a callback function that can be used with Arra.reduce()
function escape(cumul, char) {
  // Assume all characters in input will be ASCII characters
  // (No emojis or symbols from other writing systems)
  return cumul
       + "\\u00"
       + char.charCodeAt(0)
             .toString(16)
             .toUpperCase()
}

// Generate a string of Unicode Escape characters, beginning
// with an opening " double quote mark and ending with the
// Line Feed character followed by a closing double quote mark.
// 
// The character `\` has the special meaning of "ignore the next
// character, or treat it in a special way". For example `\n`
// means "new line".
//
// To include the character `\` in a string, it must be _escaped_
// with another `\` character. In other words: `\\` means "ignore"
// the fact that `\` means ignore the next character, and use the
// second `\` character exactly as it is.
//
// When used as the text for the
// workbench.action.terminal.sendSequence command, the Line Feed
// character will tell the Terminal to execute the preceding text.
const escaped = charArray.reduce(escape, '"') + '\\u000A\"'

console.log(`
### Paste the following JSON block into keybindings.json

,
{
    "key": "ctrl+shift+alt+X", // use your own shortcut
    "description": "Runs '${input}' in the  Terminal",
    "command": "workbench.action.terminal.sendSequence",
    "args": {
        "text": ${escaped}
    }
}`)
```

4. Read the code and the comments, and check that you can understand what the code does.

5. In your terminal, run `node createKeyBinding.js`.

You should see something like this in your Terminal.
```bash-#w#
## Paste the following JSON block into keybindings.json

,
{
    "key": "ctrl+shift+alt+X", // use your own shortcut
    "description": "Runs 'npm run pandoc' in the  Terminal",
    "command": "workbench.action.terminal.sendSequence",
    "args": {
        "text": "\u006E\u0070\u006D\u0020\u0072\u0075\u006E\u0020\u0070\u0061\u006E\u0064\u006F\u0063\u000A"
    }
}
```

6. Compare this output with the JSON block that you used for `npm run pandoc` in the `keybindings.json` file... except for the `p` that I used for the keyboard shortcut. Now you know where it came from.
   
## Create your own keybinding

If you find that your are running the same command in the Terminal very often, you can use this script to generate the JSON block that you need to add to your `keybindings.json` file.

For example:

1. Run the command `node createKeyBinding.js echo Keybindings are cool!`

Here's what you should see in the Terminal:

```bash-#w
<b>node createKeyBinding.js echo Keybindings are cool!</b>
### Paste the following JSON block into keybindings.json

,
{
    "key": "ctrl+shift+alt+X", // use your own shortcut
    "description": "Runs \'echo Keybindings are cool!'\ in the  Terminal",
    "command": "workbench.action.terminal.sendSequence",
    "args": {
        "text": "\u0065\u0063\u0068\u006F\u0020\u004B\u0065\u0079\u0062\u0069\u006E\u0064\u0069\u006E\u0067\u0073\u0020\u0061\u0072\u0065\u0020\u0063\u006F\u006F\u006C\u0021\u000A"
    }
}
```
Now you can:

2. Copy this JSON block and paste it into your `keybindings.json` file
3. Customize the shortcut that you want to use
4. Save your `keybindings.json` file
5. Press your chosen shortcut combination
6. Check the output in your Terminal. Does it look like this?

```bash-#w
<b>echo Keybindings are cool!</b>
Keybindings are cool!
```
Agreed, this is not a very useful command, so you can delete it after you've finished playing with it. In the future, though, you can find your `createKeyBinding.js` file in the place where you keep stuff that might be useful later, and use it to make repetitive tasks easier. 

> It's time to get back to converting Markdown and YAML to HTML.
</section>
