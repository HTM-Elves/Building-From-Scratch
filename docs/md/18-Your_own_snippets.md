<section
  id="activating-snippets"
  data-item="Activating Snippets">
  <h2>Activating Markdown Snippets</h2>
  
By default, VS Code snippets are disabled in Markdown files. You'll probably need to activate them before you can continue.

1. Open the Command Palette (`Ctrl-Shift-P` or `F1`, remember?)
2. Start typing `user settings`, until you see the option for `Preferences: Open User Settings (JSON)`
3. If this is the first item, you can press Enter to select it. Otherwise you can use the arrow buttons on your keyboard to move the selection highlight before you press enter. (Or you can use your mouse... if you think that that will be quicker.)

![User Settings](images/UserSettings.webp)

4. The `settings.json` file should open. Use the Find dialog to search for `markdown`. (Press `Ctrl-F`, for "Find" if you don't see the Find dialog.)

![quickSuggestions](images/quickSuggestions.webp)

5. You should see a JSON block with an entry for `"editor.quickSuggestions"`.
   
    ```json-#
    <i>"[markdown]": {
       <s><!-- Some lines skipped --></s>
       <u>"editor.quickSuggestions":</u> {
         "other": </i><b>"on"</b><i>
      }
    }</i>
    ```
    
6. Set the value of `"other"` to `"on"`.

> Now you can add some custom snippets for Markdown.

</section>