<section
  id="installations"
  data-item="Installations"
>
  <h2>Installing NodeJS, `npm` and `pandoc-filter`</h2>



When you ask a browser to do anything more complex than displaying a web page and its contents, a programming language called JavaScript will be at work. NodeJS is a standalone program that runs JavaScript code in the same way that JavaScript is run in the Google Chrome browser.

Basically, NodeJS is the programming-language part of Google Chrome. However, as a standalone program, it has additional features. For instance, it can read and write directly from your computer's hard disk, and it can run _node modules_ written in JavaScript, which can provide tools for specific jobs.

> Node modules are also known as Node packages. I will use both terms with the same meaning in this tutorial.

You can think of NodeJS like a food mixer which has the power to turn an drive shaft, and node modules as the different attachments that you can attach to the drive shaft, to allow you to whisk eggs or chop up carrots or knead bread dough.

Before you can continue, you are going to need to:

1. [Install NodeJS]((https://www.freecodecamp.org/news/node-version-manager-nvm-install-guide/)) on your computer. The best way to do this is with Node Version Manager (NVM)
2. [Install a program called Node Package Manager](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) (or more precisely [`npm`]([Npm](https://www.npmjs.com/package/npm#user-content-faq-on-branding))).
3. Use `npm` to [install a package called `pandoc-filter`](https://www.npmjs.com/package/pandoc-filter). In a Terminal window open on your `Workflow/` directory, run this command:
```bash-#
npm install pandoc-filter
```

You should see that a new folder called `node_modules` has been created, and two new files called `package.json` and `package-lock.json` have appeared. The contents of your `Workflow/` directory should now look something like this:
```console-#
.
├── input.md
├── node_modules
│   ├── get-stdin/
│   └── pandoc-filter/
├── index.html
├── package-lock.json
├── package.json
└── template.html
```

If that's what you see, then you are ready to continue.
</section>