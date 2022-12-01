# Lab 5: Dynamic Data (M365 Conference)

In this lab, we will retrieve data from a list and generate a dynamic logo with it.

## Exercise 1

Let's change the `IJarbisWebPartProps` interface to cache all the values we retrieve from the list; that way, we will not need to reload the data every time the web part is displayed -- only when we force generate a new logo.

1. In `JarbisWebPart.ts`, change the `IJarbisWebPartProps` and renamed the `description` property to `name`, as follows:

    ```typescript
    export interface IJarbisWebPartProps {
      name: string;
    }
    ```

1. Using the example of the `name` property, add the following properties to the `IJarbisWebPartProps` interface. All properties will be `string` typed.
  
    - `primaryPower`
    - `secondaryPower`
    - `foregroundColor`
    - `backgroundColor`
    - `foregroundIcon`
    - `backgroundIcon`

    Your final `IJarbisWebPartProps` should look as follows:

    ```typescript
    export interface IJarbisWebPartProps {
      name: string;
      primaryPower: string;
      secondaryPower: string;
      foregroundColor: string;
      backgroundColor: string;
      foregroundIcon: string;
      backgroundIcon: string;
    }
    ```

1. The `.manifest.json` file for a web part defines the values that a web part should use when a new web part is dropped on a page. Let's set some default values so that our users don't get an error when they first use our web part. In the `JarbisWebPart.manifest.json`, find the `properties` node, located under `preConfiguredEntries` and replace the `description` property (which is no longer used) with the following default values:

    ```json
    "name": "Lab Rat",
    "primaryPower": "Science",
    "secondaryPower": "Experiments",
    "foregroundColor": "orange",
    "backgroundColor": "skyblue",
    "foregroundIcon": "TestBeakerSolid",
    "backgroundIcon": "StarburstSolid"
    ```

1. Back in the `JarbisWebPart.ts` file, let's change the `render` method. Replace the static value of `The Something Hero` with `The ${this.properties.name}`
1. Replace the `Primary` power with `${this.properties.primaryPower}` (don't overwrite the opening parenthesis)
1. Replace the `Secondary` power with `${this.properties.secondaryPower}` (don't overwrite the closing parenthesis)
1. The full line should now look as follows:

    ```typescript
    (${this.properties.primaryPower} + ${this.properties.secondaryPower})
    ```

1. Replace the hard-coded `skyblue` with `${this.properties.backgroundColor}`
1. Replace the `orange` with `${this.properties.foregroundColor}`
1. Replace `'ShieldSolid'` with `this.properties.backgroundIcon`
1. Replace `'FavoriteStarFill'` with `this.properties.foregroundIcon`
1. The final `render` method should look as follows:

    ```typescript
     public render(): void {
        const hero = `
          <div class="${styles.logo}">
            <i class="${css(styles.background, getIconClassName(this.properties.backgroundIcon))}" style="color:${this.properties.backgroundColor};"></i>
            <i class="${css(styles.foreground, getIconClassName(this.properties.foregroundIcon))}" style="color:${this.properties.foregroundColor};"></i>
          </div>
          <div class="${styles.name}">
            The ${this.properties.name}
          </div>
          <div class="${styles.powers}">
            (${this.properties.primaryPower} + ${this.properties.secondaryPower})
          </div>`;
        
        this.domElement.innerHTML = `
          <div class="${styles.jarbis}">
            ${hero}
          </div>`;
      }
    ```

1. If you had `gulp serve` running and simply refresh the page, you may see that some of the values return as `undefined`. This is because the web part was added to the page before the `.manifest.json` was updated and the web part rebuilt. You'll need to stop and start `gulp serve` and remove the current instance of the web part, then add a new instance, the default values should now appear -- instead of `undefined`.

## Exercise 2

Let's demonstrate how the values are dynamically bound by using the property pane.

1. In the `JarbisWebPart.ts` file, find the `getPropertyPaneConfiguration` method -- this is the method that every web part uses when asked to display the configuration property pane.
1. The `PropertyPaneTextField` is currently bound to the `description` property, which isn't used. Change it to bind to the `foregroundIcon` instead.
1. Change the `label` property to `"Foreground icon"`
1. Add another `PropertyPaneTextField` to bind to the `primaryPower` and set the label to `"Primary Power"`.
1. The full `getPropertyPaneConfiguration` should now look as follows:

   ```typescript
   
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
   ```

1. Run `gulp serve` and try changing some of the web part properties. For example, try entering `TestBeaker` or `TestCase` in the **Foreground icon** field. You should see the web part refresh every time you change the icon.
1. Try adding another instance of the web part on the page and change the settings for that web part. You'll notice that the settings of each web part are independent from each other.

## Exercise 3

As a general rule, you should always verify input provided by users to ensure that malicious code is not executed by your web parts.

In this exercise, we'll demonstrate why you should always escape values that can be stored or entered by users before you display them on the screen.

1. While still running `gulp serve`, change the **Primary Power** of any of your currently added web parts to `<div style='color:green'>Science</div>` and observe what happens in the page.
1. Injecting custom HTML is arguably tame, but the same technique could be used to inject custom JavaScript for malicious intent.
1. At the top of the `JarbisWebPart.ts`, insert the following import:

   ```typescript
   import { escape } from '@microsoft/sp-lodash-subset';
   ```

1. Replace the code where you display the value of the `primaryPower` (`${this.properties.primaryPower}`) to call the `escape` function before rendering the value with the following code:

   ```typescript
   ${escape(this.properties.primaryPower)}
   ```

1. Refresh the web part. You'll notice that the "malicious HTML" is now escape and rendered as harmless text instead of HTML.
1. Try escaping all displayed properties. The final `render` method should look as follows:

    ```typescript
    public render(): void {
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
        
        this.domElement.innerHTML = `
            <div class="${styles.jarbis}">
            ${hero}
            </div>`;
    }
    ```
