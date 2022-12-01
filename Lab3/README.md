# Lab 3: Introduction to SCSS in SPFx (M365 Conference)

Now it's time to talk about what we're actually drawing (HTML) and how it gets all pretty (CSS)!

## Exercise 1: Basic styles

SPFx uses SCSS files for style modules. These allow you to write fancy CSS!

1. From Visual Studio Code, open `JarbisWebPart.ts`
1. Look for the `render` method
1. Replace the body of the render method with the following code:

    ```javascript
      public render(): void {
        this.domElement.innerHTML = `
          <div class="${styles.jarbis}">
            <div>
              Logo
            </div>
            <div>
              The Something Hero
            </div>
            <div>
              (Primary + Secondary)
            </div>
          </div>`;
      }
     ```

1. In each of the `<div>` that are without a class, add a `class="${ styles.yourclassgoeshere }"` attribute
1. Rename each `yourclassgoeshere` respectively to `logo`, `name`, `powers`. Your divs should look like `<div class="${styles.logo}">`, `<div class="${styles.name}">`, and `<div class="${styles.powers}">`.
1. Open the `JarvisWebPart.module.scss` and replace the content with:

    ```scss
    @import '~@microsoft/sp-office-ui-fabric-core/dist/sass/SPFabricCore.scss';
    
    .jarbis {
    
      .logo {
        color: inherit;
      }
      
      .name {
        color: inherit;
      }
      
      .powers {
        color: inherit;
      }
    }
    ```

1. If you aren't already doing so, run `gulp serve --nobrowser` and refresh the page to see your changes.


## Exercise 2: Fancy styles

1. For the next few steps, make the changes to the `JarvisWebPart.module.scss`, save your changes and monitor how it affects your web part by refreshing your page.
1. To the `.jarbis` class, add the following css code: 

    ```scss
      color: $ms-color-neutralPrimary;
      display: flex;
      flex-direction: column;
      align-items: center;
    ```

1. To the `.name` class, replace the existing style with:

   ```scss
    font-weight: bold;
    font-size: 18px;
   ```

1. To the `.powers` class, replace the existing style with:

   ```scss
    color: ms-color-neutralSecondary;
    font-size: 14px;
   ```
