# Lab 7

In the previous lab, we added a useless button. In this lab, we're wiring up the button event handler to make it more useful!

## Exercise 1

1. In the `JarbisWebPart.module.scss`, add the following code below the `.powers` block:
   
   ```scss
    .generateButton {
        margin: 12px;
        background-color: $ms-color-themePrimary;
        color: $ms-color-white;
        border: none;
        border-radius: 2px;
        height: 32px;
        font-size: 14px;
        padding: 0 12px;
        font-weight: 600;
      
        &:hover {
          background-color: $ms-color-themeDarkAlt;
        }
        &:active {
          background-color: $ms-color-themeDark;
        }
    }
    ```
1. Your finished `JarbisWebPart.module.scss` should look like this:

    ```scss
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
    
        .generateButton {
          margin: 12px;
          background-color: $ms-color-themePrimary;
          color: $ms-color-white;
          border: none;
          border-radius: 2px;
          height: 32px;
          font-size: 14px;
          padding: 0 12px;
          font-weight: 600;
      
          &:hover {
            background-color: $ms-color-themeDarkAlt;
          }
      
          &:active {
            background-color: $ms-color-themeDark;
          }
        }
    }
    ```

1. In the web part's `render` method, change the `const generateButton` line to look as follows:

    ```typescript
     const generateButton = `<button class="${styles.generateButton}">Generate</button>`;
    ```

1. Refresh your browser and admire the beautiful fancy button.