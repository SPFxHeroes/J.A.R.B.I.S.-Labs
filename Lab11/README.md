# Lab 11: Configurable Web Parts

In this lab, we'll make the web part configurable.

## Exercise 1

1. Add a new property to the `IJarbisWebPartProps` interface, make it a `boolean` and call it `powersVisible`, by using the following code:

   ```typescript
   // Indicates if the hero's powers should be shown at render time
   powersVisible: boolean;
   ```

1. In the `render` method, above the `const generateButton` line, add the following code:

   ```typescript
    const powers = `
      <div class="${styles.powers}">
        (${escape(this.properties.primaryPower)} + ${escape(this.properties.secondaryPower)})
      </div>`;
   ```

1. In the `render` method, replace the `const hero` code with the following:

   ```typescript
       const hero = `
      <div class="${styles.logo}">
        <i class="${css(styles.background, getIconClassName(escape(this.properties.backgroundIcon)))}" style="color:${escape(this.properties.backgroundColor)};"></i>
        <i class="${css(styles.foreground, getIconClassName(escape(this.properties.foregroundIcon)))}" style="color:${escape(this.properties.foregroundColor)};"></i>
      </div>
      <div class="${styles.name}">
        The ${escape(this.properties.name)}
      </div>`;
   ```

    (You'll notice we removed the list of powers from the default hero rendering)

1. Again in the `render` method, just below the `{hero}` line, insert the following code:

   ```typescript
   ${this.properties.powersVisible ? powers : ""}
   ```

   Making the completed `render` method look as follows:

   ```typescript
    public render(): void {
        const oldbuttons = this.domElement.getElementsByClassName(styles.generateButton);
        for (let b = 0; b < oldbuttons.length; b++) {
            oldbuttons[b].removeEventListener('click', this.onGenerateHero);
        }
    
        if (this.displayMode == DisplayMode.Edit && typeof this._powers == "undefined") {
            this.context.statusRenderer.displayLoadingIndicator(this.domElement, 'options');
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
            </div>`;
    
        const powers = `
            <div class="${styles.powers}">
            (${escape(this.properties.primaryPower)} + ${escape(this.properties.secondaryPower)})
            </div>`;
    
        const generateButton = `<button class="${styles.generateButton}">Generate</button>`;
    
        this.domElement.innerHTML = `
            <div class="${styles.jarbis}">
            ${hero}
            ${this.properties.powersVisible ? powers : ""}
            ${this.displayMode == DisplayMode.Edit ? generateButton : ""}
            </div>`;
    
        const buttons = this.domElement.getElementsByClassName(styles.generateButton);
        for (let b = 0; b < buttons.length; b++) {
            buttons[b].addEventListener('click', this.onGenerateHero);
        }
    }
   ```


1. At the top of **JarbisWebPart.ts**, change the following `import` statement, from this:

   ```typescript
   import {
      IPropertyPaneConfiguration,
      PropertyPaneTextField
    } from '@microsoft/sp-property-pane';
   ```

   To this:

   ```typescript
    import {
      IPropertyPaneConfiguration,
      PropertyPaneToggle
    } from '@microsoft/sp-property-pane';
   ```

1. Replace the entire `getPropertyPaneConfiguration` method with the following code:

   ```typescript
   protected getPropertyPaneConfiguration(): IPropertyPaneConfiguration {
        return {
          pages: [
            {
              groups: [
                {
                  groupFields: [
                    PropertyPaneToggle('powersVisible', {
                      label: "Powers",
                      onText: "Visible",
                      offText: "Hidden"
                    })
                  ]
                }
              ]
            }
          ]
        };
      }
   ```

1. Set the default value for `powersVisible` to `true` in the **JarbisWebPart.manifest.json** with the following code:

   ```typescript
   "powersVisible": true
   ```

   (We don't need to remind you about adding a comma to the previous line, do we?)

   The completed `properties` node should look as follows:

   ```typescript
   "properties": {
      "name": "Lab Rat",
      "primaryPower": "Science",
      "secondaryPower": "Experiments",
      "foregroundColor": "orange",
      "backgroundColor": "skyblue",
      "foregroundIcon": "TestBeakerSolid",
      "backgroundIcon": "StarburstSolid",
      "list": "Powers",
      "powersVisible": true
    }
   ```

1. In the workbench, remove the web part. Stop `gulp serve --nobrowser` and restart it, refresh the workbench page and re-add the web part (otherwise, the new default value from the manifest will not apply).
1. Try configuring the web part to hide and show the powers.

## Exercise 2

You may have noticed that changes to the property pane are immediately reflected in the web part; there may be times when you may not wish to automatically re-render the web part (for example, if a setting requires a significant amount of processing or if you have some settings that are not dependent of others).

To disable reactive property panes, you simply need to add the following code above the `getPropertyPaneConfiguration` method:

  ```typescript
  protected get disableReactivePropertyChanges(): boolean { 
    return true; 
  }
  ```

1. Test your web part again: you should see a new **Apply** button at the bottom of the property pane. Pressing it should cause the changes you made to the settings to re-render the web part.
1. When you're done testing, remove the `disableReactivePropertyChanges` code.
