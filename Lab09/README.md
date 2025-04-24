# Lab 9: Retrieving Data

In this lab, we'll retrieve data from the SharePoint list that we provisioned when the solution was deployed to a site.

To make retrieving data easier, we'll use [PnPjs](https://pnp.github.io/pnpjs/) to retrieve the data.

> :bulb: PnPjs is a collection of fluent libraries for consuming SharePoint, Graph, and Office 365 REST APIs in a type-safe way. You can use it within SharePoint Framework, Nodejs, or any JavaScript project.
>
> In fact, we recommend *always* using PnPjs when retrieving data from SharePoint or Graph. Not only does it greatly simplify batching and caching, it can validate your endpoints at build rather than at runtime because you're using a fluent API to access them rather than relying on error-prone string concatenation of endpoints, filters, etc.

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

  1. [Setup the data model](#rocket-exercise-1-setup-the-data-model)
  1. [Install and configure PnPjs](#rocket-exercise-2-install-and-configure-pnpjs)
  1. [Caching our requests](#rocket-exercise-3-caching-our-requests)
  1. [Adding a status indicator and only loading on demand](#rocket-exercise-4-adding-a-status-indicator-and-only-loading-on-demand)
</details>

<details>
<summary><b>Starter Code</b></summary>

If you skipped the previous step, or just want to start here, you can find the code ready to go in the [Lab 09 Starter](https://github.com/SPFxHeroes/J.A.R.B.I.S./tree/Start-of-Lab-09) branch.

</details>

## :rocket: Exercise 1: Setup the data model

When we retrieve and work with items from a datasource, in this case SharePoint, we want to be able to work with the results in a strongly typed way. This makes for better code, compile time validation, intellisense integration, etc.

We do this through interfaces

1. In VSCode, add a new file named **IPowerItem.ts** in the same folder as **JarbisWebPart.ts**
1. Paste the following code into the newly-created file:

    ```typescript
    /**
    * A record from the "Powers" SharePoint list.
    *
    * Represents a single record retrieved from the "Powers" SharePoint list, with properties for each of the columns in the list.
    *
    * @export
    * @interface IPowerItem
    */
    export interface IPowerItem {
           /**
         * The title of the power.
         *
         * This is the title of the record in the SharePoint list.
         *
         * @type {string}
         * @memberof IPowerItem
            */
           Title: string;
    
           /**
         * The supported colors for the power.
         *
         * This is the list of colors that will work well with this power.
         * @type {string[]}
         * @memberof IPowerItem
            */
           Colors: string[];
    
           /**
         * The icons for the power.
         *
         * This is the list of potential icons that can be used for this power.
         *
         * @type {string[]}
         * @memberof IPowerItem
            */
           Icon: string[];
    
           /**
         * The main text for the power.
         *
         * Contains a list of potential main text for the power.
         *
         * @type {string[]}
         * @memberof IPowerItem
            */
           Main: string[];
    
           /**
         * The prefix for the power.
         *
         * Contains a list of potential prefixes (e.g. Captain, Doctor, etc.) for the power.
         *
         * @type {string[]}
         * @memberof IPowerItem
            */
           Prefix: string[];
    }
    ```

    > You'll notice that we're starting to add comments to our code; this is not only to teach you how the code works as you're using it in the labs, but also to impart some coding best-practices.
    >
    > The weird comment syntax (`/**` and `*/`) we're using follows the [JSDoc](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) convention, which VSCode recognizes and will use to provide tooltips as you refer to the `IPowerItem` interface throughout the code.
    >
    > Neato!!!

1. Back in **JarbisWebPart.ts**, add an import to the newly-created `IPowerItem` interface by appending the following code at the bottom of all the `import` statements:

   ```TypeScript
   import { IPowerItem } from './IPowerItem';
   ```

 1. Try to hover your mouse over `IPowerItem` in the line you just added; you should see that the comments from the `IPowerItem` interface are reflected in the tooltips.

 1. Inside the `JarbisWebPart` class, above the `render` method, add a variable to store an array of `IPowerItem` after they have been loaded from SharePoint by adding the following code:

    ```TypeScript
    private powers: IPowerItem[];
    ```

1. In the `IJarbisWebPartProps` interface located just under the imports in the **JarbisWebPart.ts** file, add a string property called `list`, which will store the name of the list where powers will be retrieved from by adding the following code:

   ```TypeScript
   // The name of the SharePoint list that contains the powers
   list: string;
   ```

   The full `IJarbisWebPartProps` interface should now look as follows:

   ```TypeScript
   export interface IJarbisWebPartProps {
     name: string;
     primaryPower: string;
     secondaryPower: string;
     foregroundColor: string;
     backgroundColor: string;
     foregroundIcon: string;
     backgroundIcon: string;
      
     // The name of the SharePoint list that contains the powers
     list: string;
   }
   ```

1. In **JarbisWebPart.manifest.json**, add a new default property under `preconfiguredEntries` and `properties`; call it `list` and make the default value `Powers`. You can do so by adding the following JSON node (don't forget to add a comma on the line above):

   ```json
   "list": "Powers"
   ```

   The complete `properties` node in **JarbisWebPart.manifest.json** should look as follows:

   ```json
   "properties": {
     "name": "Lab Rat",
     "primaryPower": "Science",
     "secondaryPower": "Experiments",
     "foregroundColor": "orange",
     "backgroundColor": "skyblue",
     "foregroundIcon": "TestBeakerSolid",
     "backgroundIcon": "StarburstSolid",
     "list": "Powers"
   }
   ```

#### :books: Resources
- [TypeScript Object Types (Interfaces)](https://www.typescriptlang.org/docs/handbook/2/objects.html)
- [JSDoc Reference](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html)


## :rocket: Exercise 2: Install and configure PnPjs

So far we've only been using either what SPFx gave us or what we've coded ourselves. What about external libraries? There are a few different ways to work with external libraries including installing them as packages, referencing them through CDNs, or even dynamically loading them on demand. Even within those there are differences based on the type of modules (or lack of modules) that they support.

We'll be demonstrating integrating packages from PnPjs (v4.x) but there are links below to help you with other libraries (like jQuery).

1. Using the terminal, add the required PnPjs modules to your solution by typing the following command:

    ```bash
    npm install @pnp/sp
    ```

    > :warning: You will have to stop serving your web part in order to run `npm install`
    
    > :bulb: Unlike the previous times we ran the `npm install` command, we omitted the `-g` parameter, because the `-g` parameter means that we want to install the node module _globally_ (i.e.: at the Operating System level); without the `-g` parameter, the node module will be installed _locally_ (i.e.: at the solution-level).
    >
    > This is because we need the `@pnp/sp` module for our solution only, not for all solutions on our system.

1. Add the following `import` statements to the top of **JarbisWebPart.ts**, below the `import {IPowerItem }` line:

    ```TypeScript
    import { spfi, SPFx } from '@pnp/sp';
    import '@pnp/sp/webs';
    import '@pnp/sp/lists';
    import '@pnp/sp/items';
    ```

    > You may wonder why adding PnPjs requires adding so many imports. The answer is simple: PnPjs provides a _lot_ of functionality, but we don't need to use _every_ single feature provided by PnPjs - otherwise the codebase for your web part would be a lot bigger and could potentially make the SharePoint page where someone added your web part render slowly and appear sluggish and sad.
    >
    > By importing _only_ the features we need (i.e. SharePoint, webs, lists, list items), we help keep the web part code base as small as possible.

1. After the `render` method, add the following method to retrieving the list of powers from SharePoint: 

   ```TypeScript
    /**
    * Gets the list of powers from SharePoint
    *
    * @private
    * @memberof JarbisWebPart
    */
    private getPowers = async (): Promise<void> => {
      const sp = spfi().using(SPFx(this.context));

      // Get the list of powers from SharePoint using the name of the library specified in the property pane
      this.powers = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'Icon', 'Colors', 'Prefix', 'Main')();

      console.log("Powers", this.powers);
    }
   ```
   > :bulb: The first line of this method initializes PnPjs. This ensures that whether our web part is hosted on the home page for a site or on a page nested multiple folders deep, the library can correctly resolve the API locations. If we were making multiple calls we'd probably set this up as a property and configure it in the `onInit` method. Examples of that can be found in the PnPjs documentation.

   > :bulb: Notice that the actual call to the REST endpoint isn't written as a string but it's built with full support for intellisense. The above code is the equivalent to writing something like `"_api/web/lists/getbytitle(${this.properties.list})/items?$top=2000&$select=Title,Icon,Colors,Prefix,Main"` and that's relatively straightforward. Imagine needing to add filters, expansions, batching, etc. PnPjs will make all that just as easy as we're seeing here (you'll see).

   > :bulb: Sometimes pasting code makes it look wonky. You can always right-click in VS Code within the editor window and select **Format Document** (or press <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>F</kbd>) to make it pretty again.

1. Now we've got a method to retrieve list items, but we actually need to call it! At the top of the `render` method just after our oldButtons cleanup code add the following:
   ```TypeScript
   //load the powers
   this.getPowers().catch((error) => console.error(error));
   ```

1. In the terminal, run `gulp serve --nobrowser` to reserve your web part. You'll also want to remove and readd it to the workbench since we changed the manifest and added a new property with a default value.

1. Open the Developer tools in your browser, using <kbd>F12</kbd> or <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>I</kbd> on your keyboard, or by using the **Settings and more** ellipsis icon, then **More tools** > **Developer Tools** then switching to the Console. You should see where we dumped the `powers` array and if you expand it you can see the properties are nicely mapped thanks to our `IPowerItem` interface. Wowee!
   ![Powers logged in the console](assets/powersintheconsole.png)
   > :bulb: Notice that the filename shown for the source of the message is not our generated bundle js file. SPFx has provided mappings to the browser so that we can see where the message occured in our actual source file! We can even click on it to see the code in the browser and set breakpoints!

#### :books: Resources
- [PnPjs Getting started with the SharePoint Framework](https://pnp.github.io/pnpjs/getting-started/#getting-started-with-sharepoint-framework)
- [Add an external library to your SharePoint client-side web part](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/basics/add-an-external-library)
- [Use existing JavaScript libraries in SharePoint Framework client-side web parts](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/guidance/use-existing-javascript-libraries)

## :rocket: Exercise 3: Caching our requests

You might be curious why we would put a call to get data *every time* we render. Yeah... that's a bad plan. In fact, if you use the workbench to switch between Preview and Edit you'll see multiple console messages for the `powers` being logged.

This might be a small call, but you shouldn't abuse people's networks nor risk your code being sluggish because you're being overly aggressive with data retrieval.

Time to implement caching! Fortunately, PnPjs makes is super easy!

1. Start by testing the web part without caching: using your browser's developer tools, switch to the **Network** tab and filter for **Fetch / XHR** requests -- which are the types of network calls your web part makes when retrieving items from the list. Since we're only dealing with a single endpoint, we can make it even easier by adding the keyword `items` to our Filter:
   ![Fetch/XHR](assets/xhr.png)

1. Refresh the workbench page

1. Refresh the page several times, making sure to note that the `items` API is called every time. Also, note how much time the call takes every time. Toggling between Preview and Edit will show additional calls as the web part rerenders. Oh my!

1. Return to **JarbisWebPart.ts** and add the following import:

   ```Typescript
   import { Caching } from "@pnp/queryable";
   ```
   > :bulb: We didn't have to install the `@pnp/queryable` package because it was marked as a dependency of the `@pnp/sp` package and was added automatically. Yipee!

1. Change the `this.powers` line in the `getPowers` method to use the following code instead:

   ```TypeScript
   // Get the list of powers from SharePoint using the name of the library specified in the property pane
   this.powers = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'Icon', 'Colors', 'Prefix', 'Main').using(Caching())();
   ```

1. Refresh the web part. You may not immediately notice a difference, but the web part only queries the list _once_ -- unless there are changes or the cache has expired.

1. To verify that the cache is used, look for a call to the `items` API in the network calls; the first time you load the web part, you should see a call to it, but refreshing the page should not cause the web part to call the API every subsequent time.
   > :bulb: Yet we are still getting the `powers` dumped to the console each time even as we toggle Preview/Edit in the workbench!

   > :bulb: We didn't specify any configuration and are just using the caching defaults (5 minute timeout). We can adjust this in our overall configuration for PnPjs to apply to all calls or we can specify this as a parameter to the `Caching` function on a per call basis!

#### :books: Resources
- [PnPjs Caching](https://pnp.github.io/pnpjs/queryable/behaviors/#caching)


## :rocket: Exercise 4: Adding a status indicator and only loading on demand

Now we're loading data and we're doing it smarterly with caching. But we've still got the issue that we're loading the data on *every* render which is just unnecessary.

We also only need to load the data when we're generating a random hero which is something we only do when in Edit mode. The intent is that the web part will pull the hero values from its property bag when the page is in Read mode.

There's also no indicator to the end user that something is being loaded behind the scenes. You can cover a lot of performance issues (many of which might be out of your control) by simply providing feedback to the user - preferably through animation.

1. In the `render` method, replace the `this.getPowers()...` call with the following:
   ```TypeScript
    if (this.displayMode === DisplayMode.Edit && this.powers === undefined) {
      this.context.statusRenderer.displayLoadingIndicator(this.domElement, 'options');

      //load the powers
      this.getPowers().catch((error) => console.error(error));
      return;
    } else {
      this.context.statusRenderer.clearLoadingIndicator(this.domElement);
    }
   ```
   > :bulb: We're now performing a check to see if both the page is in Edit mode and that the `powers` haven't been loaded by seeing if the array is `undefined` (which is different than seeing if it's empty). This stops our unnecessary loading every time we render and prevents us from doing data retrieval at all unless the page is being edited.

   > :bulb: SPFx provides a nice utility class to show a standard loading indicator so that you don't have to implement that yourself each time. You simply ask it to show and provide the element it should show up in (we're just doing the whole thing in this case) along with the name of what's loading (options in our case).

1. Refresh to workbench and you'll spot an issue: The web part will load but never seem to stop loading. However, if you change the workbench from **Edit** mode to **Preview** mode, you'll see that the web part renders properly.

   This is because the web part does not re-render once the data is loaded!

1. Let's fix the issue by adding a call to the web part's `render` method at the end of the `getPowers`, after the data is loaded, with this code:
   
   ```TypeScript
   // Re-render the web part
   this.render();
   ```

1. Refresh the web part while the workbench is in **Edit** mode and you should see the web part render once the data has been loaded. Depending on how fast things render, you may not even notice the loading spinner.

If you run into any trouble or don't really want to do the steps above, you can just replace the entire contents of the **JarbisWebPart.ts** file with the following:

<details>
<summary>:hedgehog: JarbisWebPart.ts</summary>

```TypeScript
import { escape } from '@microsoft/sp-lodash-subset';
import { Version, DisplayMode } from '@microsoft/sp-core-library';
import {
  IPropertyPaneConfiguration,
  PropertyPaneTextField
} from '@microsoft/sp-property-pane';
import { BaseClientSideWebPart } from '@microsoft/sp-webpart-base';
import { IReadonlyTheme } from '@microsoft/sp-component-base';

import styles from './JarbisWebPart.module.scss';
import icons from './HeroIcons.module.scss';
import * as strings from 'JarbisWebPartStrings';

import { IPowerItem } from './IPowerItem';
import { spfi, SPFx } from '@pnp/sp';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/items';
import { Caching } from "@pnp/queryable";

export interface IJarbisWebPartProps {
  name: string;
  primaryPower: string;
  secondaryPower: string;
  foregroundColor: string;
  backgroundColor: string;
  foregroundIcon: string;
  backgroundIcon: string;

  // The name of the SharePoint list that contains the powers
  list: string;
}

export default class JarbisWebPart extends BaseClientSideWebPart<IJarbisWebPartProps> {

  private powers: IPowerItem[];

  public render(): void {
    const oldbuttons = this.domElement.getElementsByClassName(styles.generateButton);
    for (let b = 0; b < oldbuttons.length; b++) {
      oldbuttons[b].removeEventListener('click', this.onGenerateHero);
    }

    if (this.displayMode === DisplayMode.Edit && this.powers === undefined) {
      this.context.statusRenderer.displayLoadingIndicator(this.domElement, 'options');

      //load the powers
      this.getPowers().catch((error) => console.error(error));
      return;
    } else {
      this.context.statusRenderer.clearLoadingIndicator(this.domElement);
    }

    const hero = `
      <div class="${styles.logo} ${icons.heroIcons}">
        <i class="${this.getIconClass(escape(this.properties.backgroundIcon))} ${styles.background}" style="color:${escape(this.properties.backgroundColor)};"></i>
        <i class="${this.getIconClass(escape(this.properties.foregroundIcon))} ${styles.foreground}" style="color:${escape(this.properties.foregroundColor)};"></i>
      </div>
      <div class="${styles.name}">
        The ${escape(this.properties.name)}
      </div>
      <div class="${styles.powers}">
        (${escape(this.properties.primaryPower)} + ${escape(this.properties.secondaryPower)})
      </div>`;

    const generateButton = `<button class=${styles.generateButton}>Generate</button>`;

    this.domElement.innerHTML = `
      <div class="${styles.jarbis}">
        ${hero}
        ${this.displayMode === DisplayMode.Edit ? generateButton : ""}
      </div>`;

    const buttons = this.domElement.getElementsByClassName(styles.generateButton);
    for (let b = 0; b < buttons.length; b++) {
      buttons[b].addEventListener('click', this.onGenerateHero);
    }
  }

  /**
  * Gets the list of powers from SharePoint
  *
  * @private
  * @memberof JarbisWebPart
  */
  private getPowers = async (): Promise<void> => {
    const sp = spfi().using(SPFx(this.context));

    // Get the list of powers from SharePoint using the name of the library specified in the property pane
    this.powers = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'Icon', 'Colors', 'Prefix', 'Main').using(Caching())();

    console.log("Powers", this.powers);
    // Re-render the web part
    this.render();
  }

  private onGenerateHero = (event: MouseEvent): void => {
    console.log('Generating!');
  }

  private getIconClass(iconName: string): string | undefined {
    const iconKey: string = "icon" + iconName;
    if (this.hasKey(icons, iconKey)) {
      return icons[iconKey];
    }
  }

  private hasKey<O extends object>(obj: O, key: PropertyKey): key is keyof O {
    return key in obj;
  }

  protected onThemeChanged(currentTheme: IReadonlyTheme | undefined): void {
    if (!currentTheme) {
      return;
    }

    const {
      semanticColors
    } = currentTheme;

    if (semanticColors) {
      this.domElement.style.setProperty('--bodyText', semanticColors.bodyText || null);
      this.domElement.style.setProperty('--link', semanticColors.link || null);
      this.domElement.style.setProperty('--linkHovered', semanticColors.linkHovered || null);
    }

  }

  protected get dataVersion(): Version {
    return Version.parse('1.0');
  }

  protected getPropertyPaneConfiguration(): IPropertyPaneConfiguration {
    return {
      pages: [
        {
          header: {
            description: strings.PropertyPaneDescription
          },
          groups: [
            {
              groupName: strings.BasicGroupName,
              groupFields: [
                PropertyPaneTextField('foregroundIcon', {
                  label: "Foreground Icon"
                }),
                PropertyPaneTextField('primaryPower', {
                  label: "Primary Power"
                })
              ]
            }
          ]
        }
      ]
    };
  }

  protected onDispose(): void {
    const oldbuttons = this.domElement.getElementsByClassName(styles.generateButton);
    for (let b = 0; b < oldbuttons.length; b++) {
      oldbuttons[b].removeEventListener('click', this.onGenerateHero);
    }
  }
}

```

</details>

#### :books: Resources
- [SPFx StatusRenderer](https://learn.microsoft.com/en-us/javascript/api/sp-webpart-base/iclientsidewebpartstatusrenderer?view=sp-typescript-latest)

## :tada: All Done!
![Great Job!](assets/GreatJob.png)

We're now pulling data from a list, we've improved performance with caching, added a loading indicator, and are loading the data only when needed. And yet the web part always renders the same hero badge, regardless of what data is loaded, because it never uses the data that we loaded! Let's go fix that!

# [Previous](../Lab08/README.md) | [Next](../Lab10/README.md)
