# Lab 2: Building a web part

Let's build a web part!!!!!

## Exercise 1: Scaffold your web part

1. From your command prompt, go to your working folder. 
1. Create a new folder called `jarbis`. You can use the File Explorer or type `md jarbis`
1. Change your current folder by using `cd jarbis`
1. Once in your new fancy folder, launch the yeoman generator by typing `yo @microsoft/sharepoint`
1. When prompted for a solution name, leave the default (**jarbis**), followed by <kbd>Enter</kbd>
1. When prompted to choose a SharePoint version, pick the only choice that's available (and ask yourself why you're even prompted to do so?)
1. When prompted about adding the solution to all sites, select **N** followed by <kbd>Enter</kbd>
1. When prompted about requiring special permissions, select **N** followed by <kbd>Enter</kbd>
1. When prompted what type of client you'd like to build, select **WebPart**
1. When prompted for a Web Part name, enter **My Events**. 
1. Provide a description, if you wish. For example `Hugo made me do this`
1. When prompted for a framework, select **React**
1. Wait for the solution to be scaffolded.
1. Go to your web browser, and navigate to your Microsoft 365 Dev Tenant (e.g.: https://yourdevtenant.sharepoint.com).
1. To view the workbench, navigate to `/_layouts/15/workbench.aspx`
1. You should get an error saying that your web part isn't running... let's fix that! 
1. Type `gulp serve`.
1. You should get a warning saying:
    ```
    Warning - [spfx-serve] When serving in HTTPS mode, a PFX cert path or a cert path and a key path must be provided, or a dev certificate must be generated and trusted. If a SSL certificate isn't provided, a default, self-signed certificate will be used. Expect browser security warnings.
    ```

## Exercise 2: Install the self-signed certificate

1. Run `gulp trust-dev-cert`
1. When prompted, accept the certificate by selecting **Yes**
1. Launch your web part by typing `gulp serve --nobrowser`
1. Wait for a message saying `Finished subtask 'reload'`
1. Go back to your workbench and refresh the page. Your web part should be available to add to your browser.
1. Test the web part. When you're done, go back to your command prompt and hit <kbd>CTRL</kbd>+<kbd>C</kbd> until it asks you if you want to **Terminate batch job (Y/N)?**. Select **y**

## Exercise 3: Customize the web part

1. From the command prompt, type `code .` to open Visual Studio Code (code) in the current folder (.)
1. Launch the terminal window by hitting <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>`</kbd>.
1. From the terminal, type `gulp serve --nobrowser`
1. Try to find in the code where it says "Welcome to SharePoint" and replace it for "Wowee!!".
1. Refresh the browser to see if your results are correct.

## Exercise 4: Update icon

Generic icons are not cool!

1. From Visual Studio Code, open the `JarbisWebPart.manifest.json` (located under `src\webparts\jarbis`)
1. Look for the `officeFabricIconFontName` and replace the `Page` value to `Robot`
1. Change the description to `Just A Rather Basic Instructional Solution`
1. Save your file
1. Launch the terminal window by hitting <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>`</kbd>.
1. From the terminal, type `gulp serve --nobrowser`
1. Refresh the browser, remove the web part and look for your new icon in your list of web parts available to add.