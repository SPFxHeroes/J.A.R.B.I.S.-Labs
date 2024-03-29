# Lab 4: Working with Icons

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

  1. [Using FluentUI Icons](#rocket-exercise-1-using-fluentui-icons)
  1. [Making icons pretty](#rocket-exercise-2-making-icons-pretty)
  1. [Making icons even prettier](#rocket-exercise-3-making-icons-even-prettier)
</details>

<details>
<summary><b>Starter Code</b></summary>

If you skipped the previous step, or just want to start here, you can find the code ready to go in the [Lab 04 Starter](https://github.com/SPFxHeroes/J.A.R.B.I.S./tree/Start-of-Lab-04) branch.

</details>

## :rocket: Exercise 1: Using FluentUI icons

FluentUI is a design language provided by Microsoft and is used all over M365. There are React controls, themes, icons, and more. It's a big topic and something we recommend looking into further.

For our purposes, we're just going to be using some of the icons. We've already seen where we could use the icons for the web part icon shown in the authoring canvas. Now let's take a look at adding them in our rendered web part!

> :warning: There is currently an issue with the referenced versions of the Fluent UI packages included in SPFx starting with SPFx v1.16. It appears that there is an unintended dependency on React that will prevent our code from running as planned. Fun!
>
> Normally you would be able to use a couple of utility classes (`initializeIcons` and `getIconClassName`) to easily reference icons. Instead, we're going to take a slightly different approach to account for the issue that affects our version of SPFx. In the end it's about the same amount of code, but it isn't particularly repeatable in your own code without building a custom icon font at https://uifabricicons.azurewebsites.net/. That's beyond the scope of this lab, but we're happy to go into details if you're curious. Just ask.

1. In **JarbisWebPart.ts**, replace the word `Logo` inside `<div class="${styles.logo}">` with the following code:

    ```typescript
    <i class=""></i>
    <i class=""></i>
    ```

1. Create a new file called **HeroIcons.module.scss** in the **src** > **webparts** > **jarbis** directory (same spot as the other files we've been working with).

1. Copy all the code below and paste it in the new **HeroIcons.module.scss** file:

   ```SCSS
   .heroIcons {
      
      .iconAirplaneSolid:before { content: "\EB4C"; }
      .iconArrivals:before { content: "\EB34"; }
      .iconAsteriskSolid:before { content: "\F34D"; }
      .iconAutoEnhanceOn:before { content: "\E78D"; }
      .iconBalloons:before { content: "\ED7E"; }
      .iconBlowingSnow:before { content: "\E9C9"; }
      .iconBugSolid:before { content: "\F335"; }
      .iconBuildDefinition:before { content: "\F6E9"; }
      .iconBullseye:before { content: "\F272"; }
      .iconBullseyeTarget:before { content: "\F5F0"; }
      .iconCake:before { content: "\ECA4"; }
      .iconCalories:before { content: "\ECAD"; }
      .iconCat:before { content: "\ED7F"; }
      .iconCircleFill:before { content: "\EA3B"; }
      .iconCircleHalfFull:before { content: "\ED9E"; }
      .iconCircleShapeSolid:before { content: "\F63C"; }
      .iconCityNext:before { content: "\EC06"; }
      .iconCityNext2:before { content: "\EC07"; }
      .iconClassroomLogo:before { content: "\EF75"; }
      .iconConstructionConeSolid:before { content: "\F339"; }
      .iconDefectSolid:before { content: "\F449"; }
      .iconDefenderTVM:before { content: "\F6B3"; }
      .iconDeveloperTools:before { content: "\EC7A"; }
      .iconDiamond:before { content: "\ED02"; }
      .iconDrop:before { content: "\EB42"; }
      .iconExerciseTracker:before { content: "\EACC"; }
      .iconFangBody:before { content: "\ECEB"; }
      .iconFavoriteStarFill:before { content: "\E735"; }
      .iconFitWidth:before { content: "\E9A7"; }
      .iconFlameSolid:before { content: "\F1F3"; }
      .iconFlashlight:before { content: "\E754"; }
      .iconFreezing:before { content: "\E9EF"; }
      .iconGlasses:before { content: "\EA16"; }
      .iconHealthSolid:before { content: "\F33F"; }
      .iconHeartFill:before { content: "\EB52"; }
      .iconHomeGroup:before { content: "\EC26"; }
      .iconLadybugSolid:before { content: "\F44A"; }
      .iconLamp:before { content: "\EB19"; }
      .iconLightningBolt:before { content: "\E945"; }
      .iconManufacturing:before { content: "\E99C"; }
      .iconMedical:before { content: "\EAD4"; }
      .iconMegaphoneSolid:before { content: "\F332"; }
      .iconMessageFill:before { content: "\EC70"; }
      .iconMicrophone:before { content: "\E720"; }
      .iconNetworkTower:before { content: "\EC05"; }
      .iconPermissionsSolid:before { content: "\F349"; }
      .iconPinnedSolid:before { content: "\F676"; }
      .iconPinSolid12:before { content: "\E352"; }
      .iconPlugSolid:before { content: "\F301"; }
      .iconRainSnow:before { content: "\E9C7"; }
      .iconRibbonSolid:before { content: "\F345"; }
      .iconRobot:before { content: "\E99A"; }
      .iconRocket:before { content: "\F3B3"; }
      .iconRunning:before { content: "\EADA"; }
      .iconSearchNearby:before { content: "\E820"; }
      .iconSetAction:before { content: "\F071"; }
      .iconShieldSolid:before { content: "\F340"; }
      .iconSnowflake:before { content: "\EB46"; }
      .iconSplit:before { content: "\EDBC"; }
      .iconSqualls:before { content: "\E9CC"; }
      .iconSquareShapeSolid:before { content: "\F63D"; }
      .iconStarburstSolid:before { content: "\F33C"; }
      .iconStreetsideSplitMinimize:before { content: "\E802"; }
      .iconTestBeakerSolid:before { content: "\F3A6"; }
      .iconThunderstorms:before { content: "\E9C6"; }
      .iconUmbrella:before { content: "\EC04"; }
      .iconWeights:before { content: "\EADB"; }
      .iconWifiWarning4:before { content: "\EB63"; }
      .iconWindDirection:before { content: "\EBE6"; }

      i {
        -moz-osx-font-smoothing: grayscale;
        -webkit-font-smoothing: antialiased;
        display: inline-block;
        font-family: 'FabricMDL2HeroIcons';
        font-style: normal;
        font-weight: normal;
        speak: none;
      }
    }

    @font-face {
      font-family: 'FabricMDL2HeroIcons';
      src: url('data:application/octet-stream;base64,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') format('truetype');
    }

    /*
      Your use of the content in the files referenced here is subject to the terms of the license at https://aka.ms/fabric-assets-license
    */
   ```
   > :bulb: The scss file above was adapted from the inline-css file generated at https://uifabricicons.azurewebsites.net/ and includes only the 69 icons we're using in this project. You could always generate a different set if you'd like to repeat this workaround in your own projects.

1. Now we've got an extra stylesheet we can reference. The HeroIcons style sheet defines an icon font using base-64 encoding (so we don't have to load a separate web font file). There is a base style (`heroIcons`) that uses the font which we'll apply to the parent element in a moment. Then there are 69 styles that simply define the unicode value of a corresponding character in our font (`icon*`).

1. Head back to **JarbisWebPart.ts** and let's add an import statement so our web part is aware of these new styles. Under the `import styles from './JarbisWebPart.module.scss` let's add this import:
   ```TypeScript
   import icons from './HeroIcons.module.scss';
   ```
   > :bulb: You might have noticed something different about our style imports compared to all the other imports. Most imports do **not** contain the file extension. At first glance, it looks like these do. However, what's actually happening is that we aren't importing the stylesheet directly. Rather we're importing a file SPFx has built for us called `HeroIcons.module.scss.ts`. These files are excluded from VS Code but can be seen in the file system:

   ![SCSS files in the file system](assets/scssfiles.png)
   >What we're importing is the mapping of CSS classes to TypeScript which is what allows us to select our classes in code (rather than using a string) and takes care of the hashing to avoid conflicts. Wowee!

1. Let's adjust our HTML in our render method to include these classes. Change the `<div class="${styles.logo}">` line to:
   ```TypeScript
   <div class="${styles.logo} ${icons.heroIcons}">
   ```
   > :bulb: Be sure to leave a space between the two classes!

1. Now let's fill in those `<i>` element class values with some icon classes. In the first element, set the `class` attribute to `${icons.iconShieldSolid}` and the second element, the `class` attribute should be `${icons.iconFavoriteStarFill}`. Your `render` method should now look like this:
   ```TypeScript
   public render(): void {
     this.domElement.innerHTML = `
      <div class="${styles.jarbis}">
        <div class="${styles.logo} ${icons.heroIcons}">
          <i class="${icons.iconShieldSolid}"></i>
          <i class="${icons.iconFavoriteStarFill}"></i>
        </div>
        <div class="${styles.name}">
          The Something Hero
        </div>
        <div class="${styles.powers}">
          (Primary + Secondary)
        </div>
      </div>`;
   }
   ```

1. Save and refresh your browser and you should have a couple of icons! Fancy!

   ![icons!](assets/basicicons.png)

#### :books: Resources
- [CSS Modules](https://github.com/css-modules/css-modules)
- [SPFx CSS Recommendations](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/css-recommendations)
- [Flicon Icon Search](https://flicon.io)
- [FluentUI Icon Font Generator](https://uifabricicons.azurewebsites.net)


## :rocket: Exercise 2: Making icons pretty

One of the nice things about working with our icons as an icon font, is that we can apply styles to them just as we would text. This makes it easy to control the size, placement, and color. So let's make these things pretty!

1. Working with the same `<i>` elements as before, add a `style` element to the first one, and set the style to: `style="color:skyblue;"`

1. Set the style of the second element `style="color:orange;"`

1. Save and refresh the browser to witness the glory of color!

   ![icons, now with color!](assets/basiciconswithcolor.png)

#### :books: Resources
- [CSS Named Colors](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color)


## :rocket: Exercise 3: Making icons even prettier

Now we can see that we use some basic inline styles and our icons will be affected just as if they were text (which technically, they are). Inline styles aren't normally recommended as things are easier to maintain as a separation of concern (code here, styles there).

Having the colors be inline and setable in code will make more sense once we start making this thing dynamic. For now, let's switch back to applying our styles through classes.

1. Open the **JarvisWebPart.module.scss** and replace the styles for the `.logo` class with:

    ```scss
    .logo {
      position: relative;
      
      .background {
        font-size: 96px;
        -webkit-text-stroke: "[theme:neutralPrimary, default: #333333]" 1.4px;
      }
      
      .foreground {
        font-size: 48px;
        -webkit-text-stroke: "[theme:neutralPrimary, default: #333333]" 1.4px;
        position: absolute;
        top: 24px;
        left: 24px;
      }
    }
    ```
    > :bulb: By now you may have noticed that unlike traditional CSS, we're nesting child styles rather than having each style have a specific selector. This is one of the benefits of SCSS. In the end, it will generate all the specific selectors for us but lets us write the styles in a far less tedious and more understandable way. It'll handle composing it all back together and we get to just use our classes.

1. Back in the **JarbisWebPart.ts** file, working with the same two `<i>` elements -- again. In the first element, adjust the `class` attribute to `${icons.iconShieldSolid} ${styles.background}`. And in the second element, set the `class` attribute to `${icons.iconFavoriteStarFill} ${styles.foreground}`. Your render method should now look like this:
   ```TypeScript
   public render(): void {
    this.domElement.innerHTML = `
      <div class="${styles.jarbis}">
        <div class="${styles.logo} ${icons.heroIcons}">
          <i class="${icons.iconShieldSolid} ${styles.background}" style="color:skyblue;"></i>
          <i class="${icons.iconFavoriteStarFill} ${styles.foreground}" style="color:orange;"></i>
        </div>
        <div class="${styles.name}">
          The Something Hero
        </div>
        <div class="${styles.powers}">
          (Primary + Secondary)
        </div>
      </div>`;
   }
   ```

6. Refresh the browser and your web part should look like this:

![Preview of the web part](assets/preview.png)

If things don't look quite right, review the `render` code above and ensure your **JarbisWebPart.module.scss** file looks like this:

<details>
<summary>:hedgehog: JarbisWebPart.module.scss</summary>

```SCSS
@import '~@microsoft/sp-office-ui-fabric-core/dist/sass/SPFabricCore.scss';

.jarbis {
  color: "[theme:bodyText, default: #323130]";
  color: var(--bodyText);
  display: flex;
  flex-direction: column;
  align-items: center;

  .logo {
    position: relative;
    
    .background {
      font-size: 96px;
      -webkit-text-stroke: "[theme:neutralPrimary, default: #333333]" 1.4px;
    }
    
    .foreground {
      font-size: 48px;
      -webkit-text-stroke: "[theme:neutralPrimary, default: #333333]" 1.4px;
      position: absolute;
      top: 21px;
      left: 21px;
    }
  }

  .name {
    font-weight: bold;
    font-size: 18px;
  }

  .powers {
    color: "[theme:neutralSecondary, default: #666666]";
    font-size: 14px;
  }
}
```

</details>

#### :books: Resources
- [Use theme colors in your SPFx customizations](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/use-theme-colors-in-your-customizations)
- [Available theme tokens and Default values](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/use-theme-colors-in-your-customizations#available-theme-tokens-and-their-occurrences)


## :tada: All Done!
![Great Job!](assets/GreatJob.png)

In our next lab, we'll look at using properties stored with the instance of your web part and use them to replace some of these hardcoded values!

# [Previous](../Lab03/README.md) | [Next](../Lab05/README.md)


<!-- 2. add the following `import` statements below `import * as strings from 'JarbisWebPartStrings';`, and above `export interface IJarbisWebPartProps`:

    ```typescript
    import { initializeIcons } from '@uifabric/icons';
    import { getIconClassName } from '@uifabric/styling';
    ```

3. Below the `import` statements you just inserted, add the following code:

    ```typescript
    initializeIcons();
    ```

4. In the first element you just inserted, set the `class` attribute to `${getIconClassName('ShieldSolid')}`
5. In the second element, set the `class` attribute to `${getIconClassName('FavoriteStarFill')}`. Your `render` method should look as follows:

   ```typescript
   public render(): void {
    this.domElement.innerHTML = `
              <div class="${styles.jarbis}">
                <div class="${styles.logo}">
                  <i class="${getIconClassName('ShieldSolid')}"></i>
                  <i class="${getIconClassName('FavoriteStarFill')}"></i>
                </div>
                <div class="${styles.name}">
                  The Something Hero
                </div>
                <div class="${styles.powers}">
                  (Primary + Secondary)
                </div>
              </div>`;
      }
   ```

6. Refresh the browser

## Exercise 2

1. Working with the same elements as before, add a `style` element to the first one, and set the style to: `style="color:skyblue;"`
1. Set the style of the second element `style="color:orange;"`
1. Refresh the browser

## Exercise 3

1. Open the **JarvisWebPart.module.scss** and replace the styles for `.logo` class with:

    ```scss
    position: relative;
    
    .background {
      font-size: 96px;
      -webkit-text-stroke: $ms-color-neutralPrimary 1.4px;
    }
    
    .foreground {
      font-size: 48px;
      -webkit-text-stroke: $ms-color-neutralPrimary 1.4px;
      position: absolute;
      top: 24px;
      left: 24px;
    }
    ```

1. Back in the **JarbisWebPart.ts** file, working the same two `i` elements -- again
2. Add the top, add the following `import` statement:

   ```typescript
   import { css } from '@uifabric/utilities';
   ```
4. In the first element, set the `class` attribute to `${css(styles.background, getIconClassName('ShieldSolid'))}`
5. In the second element, set the `class` attribute to `${css(styles.foreground, getIconClassName('FavoriteStarFill'))}`
6. Refresh the browser

## Exercise 4

Let's make sure that your code is up to date with our lab!

Your **JarbisWebPart.ts** class should look like this (feel free to copy and paste over your existing code):

```typescript
import { Version } from '@microsoft/sp-core-library';
import {
  IPropertyPaneConfiguration,
  PropertyPaneTextField
} from '@microsoft/sp-property-pane';
import { BaseClientSideWebPart } from '@microsoft/sp-webpart-base';
import { escape } from '@microsoft/sp-lodash-subset';

import styles from './JarbisWebPart.module.scss';
import * as strings from 'JarbisWebPartStrings';
import { initializeIcons } from '@uifabric/icons';
import { getIconClassName } from '@uifabric/styling';
import { css } from '@uifabric/utilities';

initializeIcons();

export interface IJarbisWebPartProps {
  description: string;
}

export default class JarbisWebPart extends BaseClientSideWebPart<IJarbisWebPartProps> {

  public render(): void {
    this.domElement.innerHTML = `
      <div class="${styles.jarbis}">
        <div class="${styles.logo}">
          <i class="${css(styles.background, getIconClassName('ShieldSolid'))}" style="color:skyblue;"></i>
          <i class="${css(styles.foreground, getIconClassName('FavoriteStarFill'))}" style="color:orange;"></i>
        </div>
        <div class="${styles.name}">
          The Something Hero
        </div>
        <div class="${styles.powers}">
          (Primary + Secondary)
        </div>
      </div>`;
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
                PropertyPaneTextField('description', {
                  label: strings.DescriptionFieldLabel
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

And your **JarbisWebPart.module.scss** should match the following code:

```css
@import '~@microsoft/sp-office-ui-fabric-core/dist/sass/SPFabricCore.scss';

.jarbis {
  color: "[theme:bodyText, default: #323130]";
  color: var(--bodyText);
  display: flex;
  flex-direction: column;
  align-items: center;

  .logo {
    position: relative;
    
    .background {
      font-size: 96px;
      -webkit-text-stroke: $ms-color-neutralPrimary 1.4px;
    }
    
    .foreground {
      font-size: 48px;
      -webkit-text-stroke: $ms-color-neutralPrimary 1.4px;
      position: absolute;
      top: 24px;
      left: 24px;
    }
  }
  
  .name {
    font-weight: bold;
    font-size: 18px;
  }
  
  .powers {
    color: ms-color-neutralSecondary;
    font-size: 14px;
  }
}
```
-->