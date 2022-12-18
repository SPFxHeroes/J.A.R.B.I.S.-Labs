# Lab 10: Rendering a badge using retrieved data

## Exercise 1

1. In the **JarbisWebPart.ts**, add a `getRandomItem` method to pick an item randomly from an array of choices. Place the following code below the `getPowers` method:

   ```typescript
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

1. Replace the `onGenerateHero` method with the following code:

   ```typescript
    /**
    * Generates a new hero with random values, ensuring that the previous values are not used.
    *
    * @param {MouseEvent} _event Unused event parameter
    * @memberof JarbisWebPart
       */
      public onGenerateHero = (_event: MouseEvent): void => {
        // Get a random power from the list of powers
        const power1: IPowerItem = this.getRandomItem(this._powers);
    
        // Get another random power from the list of powers, excluding the first power
        const power2: IPowerItem = this.getRandomItem(this._powers, power1);
        
        // Get the titles each the first and second powers
        this.properties.primaryPower = power1.Title;
        this.properties.secondaryPower = power2.Title;
    
        // Get a random color for the background and foreground, excluding the colors from the first and second powers
        this.properties.backgroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors]);
        this.properties.foregroundColor = this.getRandomItem([...power1.Colors, ...power2.Colors], this.properties.backgroundColor);
    
        // Get a random icon for the background and foreground, excluding the icons from the first and second powers
        this.properties.backgroundIcon = this.getRandomItem(['StarburstSolid', 'CircleShapeSolid', 'HeartFill', 'SquareShapeSolid', 'ShieldSolid']);
        this.properties.foregroundIcon = this.getRandomItem([...power1.Icon, ...power2.Icon], this.properties.backgroundIcon);
    
        // Get a random prefix and main power, excluding the prefix from the first and second powers
        const prefix = this.getRandomItem([...power1.Prefix, ...power2.Prefix]);
        const main = this.getRandomItem([...power1.Main, ...power2.Main], prefix);
        
        // Store the name of the hero
        this.properties.name = prefix + ' ' + main;
    
        // Re-render the web part
        this.render();
      }
   ```

1. Test the web part, making sure to select **Generate** to see some generated hero names.