# Lab 11: Publish the Web Part

Our Wordle web part is feature-complete! Time to bundle it up and deploy it to SharePoint for real.

Back in [Lab 7](../Lab07/README.md) we went through the full process of bundling, packaging, uploading to the app catalog, and adding the solution to a site. The process is the same here, but with one important difference: this time we're making a **production build**.

<details>
<summary><b>Legend</b></summary>

|Icon|Meaning|
|---|---|
|:rocket:|Exercise|
|:apple:|Mac specific instructions|
|:shield:|Admin mode required|
|:bulb:|Hot tip!|
|:hedgehog:|Code catch-up|
|:warning:|Caution!|
|:books:|Resources|

</details>

<details>
<summary><b>Exercises</b></summary>

  1. [Build and package for production](#rocket-exercise-1-build-and-package-for-production)
  1. [Deploy to SharePoint](#rocket-exercise-2-deploy-to-sharepoint)
</details>


## :rocket: Exercise 1: Build and package for production

1. If your `heft start` is still running in the terminal, hit <kbd>CTRL</kbd>-<kbd>C</kbd> to stop it.

1. Build the solution for production:

   ```bash
   heft build --production
   ```

   > :bulb: The `--production` flag tells the build system to minify and optimize the JavaScript output. In [Lab 7](../Lab07/README.md), we skipped this flag because we just needed the package structure to test provisioning. Now that we're deploying the finished web part, we want the production-optimized bundle. This results in smaller file sizes and faster load times for your users.

1. Package the solution:

   ```bash
   heft package-solution --production
   ```

#### :books: Resources
- [Build and deploy your SPFx solution](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/get-started/serve-your-web-part-in-a-sharepoint-page)


## :rocket: Exercise 2: Deploy to SharePoint

From here, follow the same deployment steps from [Lab 7, Exercise 5](../Lab07/README.md#rocket-exercise-5-deploy-and-provision-to-a-site):

1. Upload the updated **wordle.sppkg** to the app catalog (it will replace the previous version)
1. Enable the app when prompted
1. The solution is already added to your site, so you don't need to add it again - just refresh!

For a more detailed walkthrough of publishing and distributing your solution (including tenant-wide deployment), refer to the publishing steps in the [J.A.R.B.I.S. workshop](https://github.com/nicknow/JARBIS-Labs).

#### :books: Resources
- [Tenant-scoped solution deployment](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/tenant-scoped-deployment)


## :tada: All Done!
![Great Job!](assets/youdidit.png)

Congratulations - you've built and deployed a fully functional Wordle game as a SharePoint web part! 🎮 From scaffolding to SCSS, property panes to PnPjs, provisioning to production deployment - you've done it all!

# [Previous](../Lab10/README.md)
