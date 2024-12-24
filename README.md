# Tutorial Template #

The command `npm run new` creates this new tutorial repository. Initially it contains the following folders, files and symlinks:
```
.
├── docs
│   ├── images
│   ├── index.html
│   └── md
│       ├── 99.md
│       └── images -> ../images
├── README.md
├── package.json
└── shared -> ../<Organization>/shared
```

If the installation is successful, a file at `docs/index.html` is generated automatically from the files in `docs/md/`, and displayed in your favourite browser.

## Writing the content of your tutorial

You can now add new Markdown files to the `docs/md/` folder and edit the existing ones.

To regenerate `docs/index.html`, you can run `npm run pandoc` in the Terminal. A better solution is to run `npm run watch` in the Terminal. This will start the `watchman` node module, and this will call `npm run pandoc` for you automatically each time you save a changed Markdown file, delete one, or create a new one.

**NOTE: You need to [install Watchman](https://facebook.github.io/watchman/docs/install) before this will work.**

To see the updated `docs/index.html` in your browser, you can run VS Code's [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer).

## Publishing your tutorial

To publish your tutorial on GitHub, you need to:

1. Create a GitHub repository for this tutorial in the GitHub Organization that you created for this purpose.
2. Commit your changes locally and push them to the remote GitHub repository.
3. Open the Settings tab in your remote GitHub repository.
4. In the Code and automation section in the left column, choose Pages
5. In the Build and Deployment section of the Pages page, choose the `main` branch and the `docs/` folder and click Save to publish the site stored in the `docs/` folder.

Assuming that your GitHub Organization is called `MyOrganization`, and your tutorial repository is called `MyTutorial`, your tutorial will be published at [https://MyOrganization.github.io/MyTutorial](). It may take a few minutes before your site goes live.

Because your tutorial repository is a child of your GitHub organization, the file at `docs/index.html` is able to access files at [https://MyOrganization.github.io/shared](). As a result, all your tutorials will have the same look and feel.

In the future, to update your tutorial, simply commit your changes and push them to the GitHub repository.

## Writing efficiently

Publishing and updating your tutorial takes only a fraction of the time that it takes to write a good tutorial. Check out the articles on [the HTM-Eleves site](https://HTM-Elves.github.io), for ideas on how to use shortcuts, keybindings and other time-saving tricks so that you can focus on writing, not on typing.

## Providing material to accompany your tutorial

You can use this repository, just like any other, to share working code and assets associated with your tutorial. Developers who clone your repository will be able to read the tutorial at `docs/index.html` locally, while reviewing and testing your code examples.

## Recycle the README

And don't forget to replace the contents of this README with a meaningful description of your own project and its tutorial, so that GitHub users will be able to find the work that you are sharing.

All the best!
