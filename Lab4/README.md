# Lab 4: Static Data

## Exercise 1

1. In the `JarbisWebPart.ts`, add the following `import` statements below all the other `import`s, and above `export interface IJarbisWebPartProps `:

    ```typescript
    import { initializeIcons } from '@uifabric/icons';
    import { getIconClassName } from '@uifabric/styling';
    import { css } from '@uifabric/utilities';
    ```

1. Below the `import` statements you just inserted, add the following code: 

    ```typescript
    initializeIcons();
    ```

1. In the code, replace the word `Logo` inside `<div class="${styles.logo}">` with the following code:

    ```typescript
    <i class=""></i>
    <i class=""></i>
    ```

1. In the first element, set the `class` attribute to `${getIconClassName('ShieldSolid')}`
1. In the second element, set the `class` attribute to `${getIconClassName('FavoriteStarFill')}`
1. Refresh the browser

## Exercise 2

1. Working with the same elements as before, add a `style` element to the first one, and set the style to: `style="color:skyblue;"`
1. Set the style of the second element `style="color:skyblue;"`
1. Refresh the browser

## Exercise 3

1. Open the `JarvisWebPart.module.scss` and replace the styles for `.logo` class with:

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

1. Back in the `JarbisWebPart.ts` file, working the same two `i` elements -- again
1. In the first element, set the `class` attribute to `${css(styles.background, getIconClassName('ShieldSolid'))}`
1. In the second element, set the `class` attribute to `${css(styles.foreground, getIconClassName('FavoriteStarFill'))}`
1. Refresh the browser
