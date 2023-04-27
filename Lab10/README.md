# Lab 10: Data-Driven Rendering

In the previous lab we started retrieving data from a SharePoint list and we did fancy things like apply caching, adding a loading indicator, set it to only load in edit mode, etc. and yet... we aren't actually doing anything with that data. Whoopsie!

Our goal is to randomize the values we're using to represent a hero using the list items as our guide. We'll then save those values in our web part's properties allowing them to be instantly retrieved when viewing the page without any need for a persistent connection to the list.

<details>
<summary><b>Legend</b></summary>

|Icon|Meaning|
|---|---|
|:rocket:|Exercise|
|:apple:|Mac specific instructions|
|:shield:|Admin mode required|
|:bulb:|Hot tip!|
|:books:|Resources|

</details>

<details>
<summary><b>Exercises</b></summary>

  1. [Hero generation](#rocket-exercise-1-hero-generation)
</details>

<details>
<summary><b>Starter Code</b></summary>

If you skipped the previous step, or just want to start here, you can find the code ready to go in the [Lab 10 Starter](https://github.com/SPFxHeroes/J.A.R.B.I.S./tree/Start-of-Lab-10) branch.

</details>

## :rocket: Exercise 1: Hero generation

1. In the **JarbisWebPart.ts**, add the following method to your web part after the `onGenerateHero` method:

   ```TypeScript
   /**
   * Gets a random value from an array of choices, excluding a specific value
   *
   * @private
   * @param {any[]} choices The array of choices to pick from
   * @param {any} exclusion The value to exclude from the choices
   * @memberof JarbisWebPart
   */

   private getRandomItem = (choices: any[], exclusion?: any): any => {
     // Filter the choices to exclude the previous value
     const filteredChoices = choices.filter((value) => value !== exclusion);

     // If there are any choices left, pick a random one
     if (filteredChoices.length) {
       return filteredChoices[Math.floor(Math.random() * filteredChoices.length)];
     }

     // Otherwise, return an empty string
     return "";
   }
   ```
   > :bulb: This method takes an array and returns a random item from it. It also takes an optional parameter that should NOT be chosen from the array. This allows us to pass a list of powers and have it pick a random one for the primary power. We can then pass that same list of powers along with the one we just picked for the primary to get the secondary power and we can be sure they aren't the same one.

1. Replace the `onGenerateHero` method with the following code:

   ```typescript
   /**
   * Generates a new hero with random values
   *
   * @param {MouseEvent} _event Unused event parameter
   * @memberof JarbisWebPart
     */
   public onGenerateHero = (_event: MouseEvent): void => {
     // Get a random power (list item) from the list of powers
     const power1: IPowerItem = this.getRandomItem(this.powers);

     // Get another random power (list item) from the list of powers, excluding the first power
     const power2: IPowerItem = this.getRandomItem(this.powers, power1);

     // Get the titles from each of the powers and save them to our properties
     this.properties.primaryPower = power1.Title;
     this.properties.secondaryPower = power2.Title;

     // Get a random color for the background choosing from the combined color suggestions for the two powers
     this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]);
     // Get a random color for the foreground choosing from the same list of suggestions but excluding the background color
     this.properties.foregroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors], this.properties.backgroundColor);

     // Get a random icon for the background choosing from a fixed list of background icons
     this.properties.backgroundIcon = this.getRandomItem(['StarburstSolid', 'CircleShapeSolid', 'HeartFill', 'SquareShapeSolid', 'ShieldSolid']);
     // Get a random icon for the foreground choosing from the combined icon suggestions for the two powers
     this.properties.foregroundIcon = this.getRandomItem([...power1.Icon, ...power2.Icon], this.properties.backgroundIcon);

     // Get the prefix choosing from the combined prefix suggestions for the two powers
     const prefix = this.getRandomItem([...power1.Prefix, ...power2.Prefix]);
     // Get the main portion of the name by choosing from the combined main suggestions
     //  for the two powers excluding the prefix since there is some overlap
     const main = this.getRandomItem([...power1.Main, ...power2.Main], prefix);

     // Store the name of the hero by combining the prefix with the main
     this.properties.name = prefix + ' ' + main;

     // Re-render the web part
     this.render();
   }
   ```
   > :bulb: The `...` you're seeing might not be super familiar to you. This is called the **spread** operator. When used preceding an array like we're seeing above it has the affect of expanding the arguments. We're using it to automatically add the items of the array to a new array and by doing it with a secondary array it's an easy way to combine them into a temporary array making it simpler to choose an item across multiple arrays. Whoo Whoo!

1. Remove that `console.log("Powers", this.powers);` line from the `getPowers` method. Microsoft pollutes the console enough as it is, no need to add to the noise!

1. Test the web part, making sure to press the **Generate** button until you're happy with your randomly generated hero!

1. Order a full body latex suit utilizing your newly generated logo.

1. Rub some radiated something or other on yourself until you begin to generate the correct powers.

1. Save the world!


If you run into any trouble or don't really want to do the steps above, you can just replace the entire contents of the **JarbisWebPart.ts** file with the following:

<details>
<summary>JarbisWebPart.ts</summary>

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
import "@pnp/sp/items/get-all";
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
    this.powers = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'Icon', 'Colors', 'Prefix', 'Main').using(Caching()).getAll();

    // Re-render the web part
    this.render();
  }

  /**
  * Generates a new hero with random values
  *
  * @param {MouseEvent} _event Unused event parameter
  * @memberof JarbisWebPart
     */
  public onGenerateHero = (_event: MouseEvent): void => {
    // Get a random power (list item) from the list of powers
    const power1: IPowerItem = this.getRandomItem(this.powers);

    // Get another random power (list item) from the list of powers, excluding the first power
    const power2: IPowerItem = this.getRandomItem(this.powers, power1);

    // Get the titles from each of the powers and save them to our properties
    this.properties.primaryPower = power1.Title;
    this.properties.secondaryPower = power2.Title;

    // Get a random color for the background choosing from the combined color suggestions for the two powers
    this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]);
    // Get a random color for the foreground choosing from the same list of suggestions but excluding the background color
    this.properties.foregroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors], this.properties.backgroundColor);

    // Get a random icon for the background choosing from a fixed list of background icons
    this.properties.backgroundIcon = this.getRandomItem(['StarburstSolid', 'CircleShapeSolid', 'HeartFill', 'SquareShapeSolid', 'ShieldSolid']);
    // Get a random icon for the foreground choosing from the combined icon suggestions for the two powers
    this.properties.foregroundIcon = this.getRandomItem([...power1.Icon, ...power2.Icon], this.properties.backgroundIcon);

    // Get the prefix choosing from the combined prefix suggestions for the two powers
    const prefix = this.getRandomItem([...power1.Prefix, ...power2.Prefix]);
    // Get the main portion of the name by choosing from the combined main suggestions
    //  for the two powers excluding the prefix since there is some overlap
    const main = this.getRandomItem([...power1.Main, ...power2.Main], prefix);

    // Store the name of the hero by combining the prefix with the main
    this.properties.name = prefix + ' ' + main;

    // Re-render the web part
    this.render();
  }

  /**
  * Gets a random value from an array of choices, excluding a specific value
  *
  * @private
  * @param {any[]} choices The array of choices to pick from
  * @param {any} exclusion The value to exclude from the choices
  * @memberof JarbisWebPart
  */

  private getRandomItem = (choices: any[], exclusion?: any): any => {
    // Filter the choices to exclude the previous value
    const filteredChoices = choices.filter((value) => value !== exclusion);

    // If there are any choices left, pick a random one
    if (filteredChoices.length) {
      return filteredChoices[Math.floor(Math.random() * filteredChoices.length)];
    }

    // Otherwise, return an empty string
    return "";
  }

  private getIconClass(iconName: string): string {
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
- [Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)
- [Math.floor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
- [Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## :tada: All Done!
![Great Job!](assets/GreatJob.png)

Our web part is looking great! However, we did some property pane experiements in an earlier lab and they don't make a whole lot of sense now. In our next lab, we'll clean those up and look at how we could provide something useful there!

# [Previous](../Lab09/README.md) | [Next](../Lab11/README.md)