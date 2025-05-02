# Lab 10: Data-Driven Rendering

In the previous lab we started retrieving data from a SharePoint list and we did fancy things like apply caching, adding a loading indicator, set it to only load in edit mode, etc. and yet... we aren't actually doing anything with that data. Whoopsie!

Our goal is to randomize the values we're using to represent a hero and we'll be using the list items as our guide. We'll then save those values in our web part's properties allowing them to be instantly retrieved when _viewing_ the page - without any need for a persistent connection to the list. Aw yeah!

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

  1. [Hero generation](#rocket-exercise-1-hero-generation)
</details>

<details>
<summary><b>Starter Code</b></summary>

If you skipped the previous step, or just want to start here, you can find the code ready to go in the [Lab 10 Starter](https://github.com/SPFxHeroes/JARBIS/tree/Start-of-Lab-10) branch.

</details>

## :rocket: Exercise 1: Hero generation

1. In the **JarbisWebPart.ts**, add the following method to your web part after the `onGenerateHero` method:

    ```TypeScript
    /**
     * Gets a random value from an array of choices, excluding a specific value
    *
    * @param choices The array of choices to pick from
    * @param exclusion The value to exclude from the choices
    */
    private getRandomItem = (choices: any[], exclusion?: any): any => {
      // Filter the choices to exclude the previous value
      const filteredChoices = choices.filter((value) => value !== exclusion);

      // If there are any choices left, pick a random one
      if (filteredChoices.length) {
        return filteredChoices[Math.floor(Math.random() * filteredChoices.length)];
      }

      // Otherwise, return undefined
      return;
    }
    ```
   > :bulb: This method takes an array and returns a random item from it. It also takes an optional parameter that should NOT be chosen from the array (excluded). This allows us to pass a list of powers and have it pick a random one for the primary power. We can then pass that same list of powers along with the one we just picked for the primary to get the secondary power and we can be sure they aren't the same one.

1. Replace the `onGenerateHero` method with the following code:

  ```typescript
  /**
   * Generates a new hero with random values
   *
   * @param _event Unused event parameter
   */
  public onGenerateHero = (_event: MouseEvent): void => {
    // Get a random power (list item) from the list of powers
    const power1 = this.getRandomItem(this.powers);

    // Get another random power (list item) from the list of powers, excluding the first power
    const power2 = this.getRandomItem(this.powers, power1);

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

1. Review the code and comments to understand what this method is doing. Here's a high-level summary:

    - Picks a random Power (item) from our retrieved list items
    - Picks another random Power (item) from our retrieved list items but ensures it isn't the same as the first one
    - Assigns property values for `primaryPower`, `secondaryPower`, `backgroundColor`, `foregroundColor`, and `foregroundIcon` using the same `getRandomItem` utility function
    - Assigns the `backgroundIcon` property value from a fixed list of good background icons using that same `getRandomItem` utility method
    - Assigns the `name` property value by combining a random prefix and main value
    - Renders the web part again to use the new properties

1. The `getRandomItem` method will work just fine as is, but you might have VS Code getting irritated with you and underlining all the use of `any` in the method signature. This is because, while `any` is allowed, it defeats a lot of the purpose of TypeScript by providing an escape hatch from the typing system that could potentially get you in trouble.

    > :bulb: This code is actually relatively safe because we know exactly how we're going to call it and the types of parameters we're going to give it. BUT that's rarely the case for long. We know how we're going to use it _now_, not how someone else might use it (even if that someone ends up being us).
    >
    > For instance, it's technically accurate to call this method with an array of numbers and pass an exclusion of type string. If you've worked with JavaScript for long, you might see where this is heading. String to number comparisons can have surprising results due to the quirkiness of how JavaScript coallesces values.
    >
    > We want to be able to use it with arrays of `IPowerItem` and arrays of `string` just in our `onGenerateHero` method alone. But if you hover over `power1` you may already spot a problem:

    ![power has a type of `any`](./assets/anypower.png)

    > The inferred type of `power1` is `any` which means we won't get intellisense or property validation when we reference sub props like `Title`. This is more than just an inconvience, it could easily lead to us not catching when interfaces change and having to track down a bug manually. Heaven forbid!
    >
    > The "obvious" solution is to stop using inferred types by specifying the type of `power1` in our definition. This would fix some of our concerns, but we're still having to assume that the utility method will be consistent meaning we are relying on the method to return an item of the same type as the array of items we passed, but it's not guaranteed and could cause hard to track down issues once you're live.

1. So, let's improve this method with some TypeScript magic. Let's review. Our goal is to keep things generic so that we can pass an array of anything and a matching exclusion. But we want to ensure that all the things in the array are the same things and that the exclusion is of the same type so that we know our comparsion will go as expected. We also want to ensure that what we return is the same type as well. We don't want no surprises none!

1. Replace the first line of the method (`private getRandomItem = (choices: any[], exclusion?: any): any => {`) with the following:

    ```TypeScript
    private getRandomItem = <T>(choices: T[], exclusion?: T): T | undefined => {
    ```

    > :bulb: By prefixing our method signature with `<T>` we are adding a Type parameter (the use of the letter `T` is just a standard convention). We can then use that Type as a generic representation of the _same type_ for consistency. So we use that in place of `any` for our array type and our exclusion type. We even use it to replace our return type. But you'll notice we've got a little something extra there (`|`) to indicate that we'll return _either_ an object of type `T` or an `undefined` value. This allows us to be very explicit about what can be expected.

1. Uh oh, now we got more problems! Look at all the errors in the `onGenerateHero` method! Actually, we have the same problems we already had but we had inadvertantly covered them up by using `any`.

    > :bulb: The `getRandomItem` method _always_ had an edge case where the return value could end up being `undefined` (no items left in the array after the exclusion item is removed). We weren't handling that in our `onGenerateHero` but TypeScript wasn't flagging that we weren't handling it because it wasn't told it was a possibility. Now that it knows, it's doing us a favor and blowing our code up until we fix it.

1. TypeScript is super smart. It just wants you to handle the `undefined` edge case. Once you've done that, it'll stop complaining. So let's add an `if` statement that will prevent us from causing errors by accessing sub props of `undefined` objects. Immediately after the `power2` add this `if` statement:

    ```TypeScript
    if (typeof power1 === 'undefined' || typeof power2 === 'undefined') {
      console.error("Unable to get powers");
      return;
    }
    ```

1. Ohhh some of the errors went away! Specifically, the use of sub properties on either `power1` or `power2`. That's because TypeScript is now satisfied that we will have ensured they are not `undefined` before we get to that point (we simply return and don't set anything).

1. But what about all the other property set calls that are still in error? Well, we said in our `IJarbisWebPartProps` interface that all of these properties were strings and not `undefined` so potentially setting them to `undefined` is problematic.

1. We can fix that by saying, if the result of the call is `undefined` then just use the value we've already got. We could write temp variables and if statements to validate each but that's no fun. Instead, we're going to use the [Nullish Coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) (`??`)

    > :bulb: The [Nullish Coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) returns the right-hand value only if the left-hand value is `null` or `undefined`; otherwise it returns the left-hand value.

1. On each of the `this.properties.` lines add the `??` operator to the end with the right-hand value being itself. For instance, change this line:

    ```TypeScript
    this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]);
    ```

    To this:

    ```TypeScript
    this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]) ?? this.properties.backgroundColor;
    ```

    > :bulb: This code will set the value of the property to the result of the `getRandomItem` method except in the unlikely event that `getRandomItem` returns `undefined` in which case it'll just use it's own value since that's already typed to be a string.

1. Your `onGenerateHero` method should now look like this:

  ```TypeScript
  /**
   * Generates a new hero with random values
   *
   * @param _event Unused event parameter
   */
  public onGenerateHero = (_event: MouseEvent): void => {
    // Get a random power (list item) from the list of powers
    const power1 = this.getRandomItem(this.powers);

    // Get another random power (list item) from the list of powers, excluding the first power
    const power2 = this.getRandomItem(this.powers, power1);

    if (typeof power1 === 'undefined' || typeof power2 === 'undefined') {
      console.error("Unable to get powers");
      return;
    }

    // Get the titles from each of the powers and save them to our properties
    this.properties.primaryPower = power1.Title;
    this.properties.secondaryPower = power2.Title;

    // Get a random color for the background choosing from the combined color suggestions for the two powers
    this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]) ?? this.properties.backgroundColor;
    // Get a random color for the foreground choosing from the same list of suggestions but excluding the background color
    this.properties.foregroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors], this.properties.backgroundColor) ?? this.properties.foregroundColor;

    // Get a random icon for the background choosing from a fixed list of background icons
    this.properties.backgroundIcon = this.getRandomItem(['StarburstSolid', 'CircleShapeSolid', 'HeartFill', 'SquareShapeSolid', 'ShieldSolid']) ?? this.properties.backgroundIcon;
    // Get a random icon for the foreground choosing from the combined icon suggestions for the two powers
    this.properties.foregroundIcon = this.getRandomItem([...power1.Icon, ...power2.Icon], this.properties.backgroundIcon) ?? this.properties.foregroundIcon;

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

1. Remove that `console.log("Powers", this.powers);` line from the `getPowers` method. Microsoft pollutes the console enough as it is, no need to add to the noise!

1. Test the web part, making sure to press the **Generate** button until you're happy with your randomly generated hero!

1. Order a full body latex suit utilizing your newly generated logo.

1. Rub some radiated something or other on yourself until you begin to generate the correct powers.

1. Save the world!


If you run into any trouble or don't really want to do the steps above, you can just replace the entire contents of the **JarbisWebPart.ts** file with the following:

<details>
<summary>:hedgehog: JarbisWebPart.ts</summary>

```TypeScript
import { Version, DisplayMode } from '@microsoft/sp-core-library';
import {
  type IPropertyPaneConfiguration,
  PropertyPaneTextField
} from '@microsoft/sp-property-pane';
import { BaseClientSideWebPart } from '@microsoft/sp-webpart-base';
import type { IReadonlyTheme } from '@microsoft/sp-component-base';

import styles from './JarbisWebPart.module.scss';
import * as strings from 'JarbisWebPartStrings';
import { getIconClassName } from '@fluentui/style-utilities';
import { css } from '@fluentui/utilities';
import { escape } from '@microsoft/sp-lodash-subset';
import { IPowerItem } from './IPowerItem';
import { spfi, SPFx } from '@pnp/sp';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/items';
import { Caching } from '@pnp/queryable';

export interface IJarbisWebPartProps {
  name: string;
  primaryPower: string;
  secondaryPower: string;
  foregroundColor: string;
  backgroundColor: string;
  foregroundIcon: string;
  backgroundIcon: string;

  /**
   * The name of the SharePoint list that contains the powers.
   */
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
      <div class="${styles.logo}">
        <i class="${css(styles.background, getIconClassName(escape(this.properties.backgroundIcon)))}" style="color:${escape(this.properties.backgroundColor)};"></i>
        <i class="${css(styles.foreground, getIconClassName(escape(this.properties.foregroundIcon)))}" style="color:${escape(this.properties.foregroundColor)};"></i>
      </div>
      <div class="${styles.name}">
        The ${escape(this.properties.name)}
      </div>
      <div class="${styles.powers}">
        (${escape(this.properties.primaryPower)} + ${escape(this.properties.secondaryPower)})
      </div>`;

    const generateButton = `<button class="${styles.generateButton}">Generate</button>`;

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
     */
  private getPowers = async (): Promise<void> => {
    const sp = spfi().using(SPFx(this.context));

    // Get the list of powers from SharePoint using the name of the library specified in the property pane
    this.powers = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'Icon', 'Colors', 'Prefix', 'Main').using(Caching())();

    // Re-render the web part
    this.render();
  }

  /**
   * Generates a new hero with random values
   *
   * @param _event Unused event parameter
   */
  public onGenerateHero = (_event: MouseEvent): void => {
    // Get a random power (list item) from the list of powers
    const power1 = this.getRandomItem(this.powers);

    // Get another random power (list item) from the list of powers, excluding the first power
    const power2 = this.getRandomItem(this.powers, power1);

    if (typeof power1 === 'undefined' || typeof power2 === 'undefined') {
      console.error("Unable to get powers");
      return;
    }

    // Get the titles from each of the powers and save them to our properties
    this.properties.primaryPower = power1.Title;
    this.properties.secondaryPower = power2.Title;

    // Get a random color for the background choosing from the combined color suggestions for the two powers
    this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]) ?? this.properties.backgroundColor;
    // Get a random color for the foreground choosing from the same list of suggestions but excluding the background color
    this.properties.foregroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors], this.properties.backgroundColor) ?? this.properties.foregroundColor;

    // Get a random icon for the background choosing from a fixed list of background icons
    this.properties.backgroundIcon = this.getRandomItem(['StarburstSolid', 'CircleShapeSolid', 'HeartFill', 'SquareShapeSolid', 'ShieldSolid']) ?? this.properties.backgroundIcon;
    // Get a random icon for the foreground choosing from the combined icon suggestions for the two powers
    this.properties.foregroundIcon = this.getRandomItem([...power1.Icon, ...power2.Icon], this.properties.backgroundIcon) ?? this.properties.foregroundIcon;

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
   * @param choices The array of choices to pick from
   * @param exclusion The value to exclude from the choices
   */
  private getRandomItem = <T>(choices: T[], exclusion?: T): T | undefined => {
    // Filter the choices to exclude the previous value
    const filteredChoices = choices.filter((value) => value !== exclusion);

    // If there are any choices left, pick a random one
    if (filteredChoices.length) {
      return filteredChoices[Math.floor(Math.random() * filteredChoices.length)];
    }

    // Otherwise, return undefined
    return;
  }

  protected onDispose(): void {
    const oldbuttons = this.domElement.getElementsByClassName(styles.generateButton);
    for (let b = 0; b < oldbuttons.length; b++) {
      oldbuttons[b].removeEventListener('click', this.onGenerateHero);
    }
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
                  label: "Foreground icon",
                }),
                PropertyPaneTextField('primaryPower', {
                  label: "Primary power",
                })
              ]
            }
          ]
        }
      ]
    };
  }
}

```

</details>


#### :books: Resources
- [Math.random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)
- [Math.floor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
- [Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [Nullish Coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)
- [TypeScript Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

## :tada: All Done!
![Great Job!](assets/GreatJob.png)

Our web part is looking great! However, we did some property pane experiments in an earlier lab and they don't make a whole lot of sense now. In our next lab, we'll clean those up and look at how we could provide something useful there!

# [Previous](../Lab09/README.md) | [Next](../Lab11/README.md)