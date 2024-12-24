<section
id="publishing-to-github"
aria-labelledby="publishing-to-github"
data-item="Publishing To GitHub"
>
<h2><a href="#publishing-to-github">Publishing To GitHub</a></h2>

1. Create a GitHub repository for this tutorial in the GitHub Organization that you created for this purpose. I'll assume that this repository has the address: `https://MyTutorials.github.com/boilerplate-tutorial`
2. On your development computer, use `git remote add origin git@github.com:MyTutorials/boilerplate-tutorial.git` to connect this local repository with the remote GitHub repository
3. Use `git push -u origin main` to push the Initial Commit of this local repository to your remote GitHub repository.
4. Open the Settings tab in your remote GitHub repository page.
5. In the Code and automation section in the left column, choose Pages
6. In the Build and Deployment section of the Pages page, choose the `main` branch and the `docs/` folder and click Save to publish the site stored in the `docs/` folder.

Your tutorial will be published at a URL like [https://MyTutorials.github.io/boilerplate-tutorial](). It may take a few minutes before your site goes live.

<details
class="pivot"
open
>
<summary>From Boilerplate to Personal Content</summary>
If all goes well, you should now find a short tutorial (that you did not write yourself) available for the world to see. Now the real work begins: working efficiently to write your own content.

</details>
</section>
