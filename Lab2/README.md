# Lab 2: Building a web part (M365 Conference)

Let's build a web part!!!!!

## Exercise 1: Scaffold your web part

1. From your command prompt, go to your working folder.
    - If you closed your command prompt, open the folder in file explorer and type `cmd` in the path to automatically open a command prompt here!
1. Create a new folder called `jarbis`. You can use the File Explorer or type `md jarbis`
    - (md stands for Make Directory)
    - (use `mkdir jarbis` on Mac)
  > **Pro Tip:** Make sure to keep your path as short as possible; Node projects can end up with very long file paths, and Windows can sometimes report unpredictable issues due to file paths being too long.
2. Change your current folder by using `cd jarbis`
    - (cd stands for Change Directory)
3. Once in your new fancy folder, launch the yeoman generator by typing `yo @microsoft/sharepoint`
4. When prompted for a solution name, leave the default (**jarbis**), followed by <kbd>Enter</kbd>
5. When prompted to choose a SharePoint version, pick the only choice that's available (and ask yourself why you're even prompted to do so)
6. When prompted about adding the solution to all sites, select **N** followed by <kbd>Enter</kbd>
7. When prompted about requiring special permissions, select **N** followed by <kbd>Enter</kbd>
8. When prompted what type of client you'd like to build, select **WebPart**
9. When prompted for a Web Part name, enter **Jarbis**. 
10. Provide a description, if you wish. For example `Hugo made me do this`
11. When prompted for a framework, select **No Framework**
12. Wait for the solution to be scaffolded.
    - (This may take a moment or 75 depending on the wifi)
13. Go to your web browser, and navigate to your Microsoft 365 Dev Tenant (e.g.: https://yourdevtenant.sharepoint.com).
14. To view the workbench, navigate to `[YOUR_ROOT_SITE_HERE]/_layouts/15/workbench.aspx`
15. You should get an error saying that your web part isn't running... let's fix that! 
16. Type `gulp serve`.
17. You should get a warning saying:
    ```
    Warning - [spfx-serve] When serving in HTTPS mode, a PFX cert path or a cert path and a key path must be provided, or a dev certificate must be generated and trusted. If a SSL certificate isn't provided, a default, self-signed certificate will be used. Expect browser security warnings.
    ```

## Exercise 2: Install the self-signed certificate

1. Run `gulp trust-dev-cert`
1. When prompted, accept the certificate by selecting **Yes**
1. Launch your web part by typing `gulp serve --nobrowser`
    - (the `--nobrowser` part keeps it from launching yet another window)
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
    - (You can find additional icons at https://flicon.io)
1. Change the description to `Just A Rather Basic Instructional Solution`
1. Save your file
1. Launch the terminal window by hitting <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>`</kbd>.
    - (You can also go to the Terminal menu and choose New Terminal)
1. From the terminal, type `gulp serve --nobrowser`
1. Refresh the browser, remove the web part and look for your new icon in your list of web parts available to add.