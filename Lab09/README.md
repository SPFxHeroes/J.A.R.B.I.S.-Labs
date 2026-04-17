# Lab 9: Random Word Selection

We're loading words from SharePoint, but we're not using them yet! In this lab, we'll pick a random word from our list when the game loads. Want a new word? Just refresh the page!

This is actually how the real Wordle works - one puzzle per day, and if you want to try again, you wait until tomorrow (or refresh 😉).

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

  1. [Pick a random word](#rocket-exercise-1-pick-a-random-word)
  1. [Remove the targetWord property](#rocket-exercise-2-remove-the-targetword-property)
</details>

## :rocket: Exercise 1: Pick a random word

Right now our `getWords` method loads the words and re-renders. Let's update it to also pick a random word!

1. First, add a class variable to store the target word. Find your other game state variables and add:

   ```typescript
   private targetWord: string = '';
   ```

   > :bulb: Up until now, the target word came from a web part property (`this.properties.targetWord`) - meaning someone had to manually type it into the property pane. That's great for testing, but not very fun for a real game! By moving it to a class variable, we can set it programmatically from our word list instead. We initialize it as an empty string and will fill it in once the words load from SharePoint.

   Your game state variables should now look like:

   ```typescript
   // Game state - stored in memory (resets on page refresh)
   private guesses: string[] = [];
   private currentGuess: string = '';
   private maxGuesses: number = 6;
   private gameStatus: string = 'playing';
   private targetWord: string = '';
   ```

1. Add a helper method to pick a random item from an array. Add this after your `getWords` method:

   ```typescript
   /**
    * Gets a random word from the loaded words
    */
   private getRandomWord(): string {
     if (this.words.length === 0) {
       return 'HELLO'; // Fallback if no words loaded
     }
     const randomIndex = Math.floor(Math.random() * this.words.length);
     return this.words[randomIndex].Title.toUpperCase();
   }
   ```

   > :bulb: Let's break down how the random selection works:
   > - `Math.random()` returns a decimal between 0 (inclusive) and 1 (exclusive) - for example `0.73`
   > - We multiply that by `this.words.length` to scale it to our array size - e.g., if we have 50 words: `0.73 * 50 = 36.5`
   > - `Math.floor()` rounds down to the nearest whole number - `36.5` becomes `36`
   > - That gives us a valid array index! We use it to grab a word and `.toUpperCase()` it so comparisons are consistent throughout the game.
   >
   > The `'HELLO'` fallback is a safety net - if the SharePoint list is empty or something went wrong loading, the game still works instead of crashing.

1. Update the `getWords` method to pick a random word after loading:

   ```typescript
   private getWords = async (): Promise<void> => {
     const sp = spfi().using(SPFx(this.context));
     this.words = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'WordCategory')();
     this.wordsLoaded = true;
     
     // Pick a random word for this game
     this.targetWord = this.getRandomWord();
     
     console.log("Words loaded:", this.words.length, "Target:", this.targetWord);
     this.render();
   }
   ```

   > :bulb: Notice the order here matters! We load the words from SharePoint first, *then* pick a random word from them, and *then* re-render. If we tried to pick a word before the list loaded, we'd always get our `'HELLO'` fallback. The `console.log` is handy during development - you can open your browser's dev tools (F12) to see which word was picked, making it easier to test that your game logic is working correctly.

1. Now we need to swap every reference from `this.properties.targetWord` to `this.targetWord`. Why? Because the word is no longer coming from the property pane configuration - it's coming from our randomly selected class variable. The next three steps are all part of this same find-and-replace effort across the web part.

   In the `renderGrid` method, this is where we color the tiles after a guess is submitted. Change:

   ```typescript
   const stateClass = guess ? this.getTileState(guess, col, this.properties.targetWord) : (letter ? styles.filled : '');
   ```

   To:

   ```typescript
   const stateClass = guess ? this.getTileState(guess, col, this.targetWord) : (letter ? styles.filled : '');
   ```

1. In the `submitGuess` method, this is the win condition check - where we compare the player's guess against the answer. Change:

   ```typescript
   if (this.currentGuess === this.properties.targetWord.toUpperCase()) {
   ```

   To:

   ```typescript
   if (this.currentGuess === this.targetWord) {
   ```

   > :bulb: Notice we also dropped the `.toUpperCase()` call! That's because `getRandomWord()` already stores the word in uppercase, so we don't need to convert it every time we compare.

1. In the `render` method, this is the "game over" message that reveals the answer when the player loses. Change:

   ```typescript
   titleText += ' 😢 (It was ' + escape(this.properties.targetWord.toUpperCase()) + ')';
   ```

   To:

   ```typescript
   titleText += ' 😢 (It was ' + escape(this.targetWord) + ')';
   ```


1. Refresh the workbench and play! Each time you refresh, you should get a different random word from your list!


## :rocket: Exercise 2: Remove the targetWord property

Since we're now picking a random word from the list, we don't need `targetWord` in our properties anymore. Let's clean that up!

1. In **WordleWebPart.ts**, update the `IWordleWebPartProps` interface to remove `targetWord`:

   ```typescript
   export interface IWordleWebPartProps {
     title: string;
     list: string;
   }
   ```

1. In **WordleWebPart.manifest.json**, remove the `targetWord` property from `preconfiguredEntries`:

   ```json
   "properties": {
     "title": "Wordle",
     "list": "WordleWords"
   }
   ```

1. In the `getPropertyPaneConfiguration` method, remove the `targetWord` TextField:

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
                 PropertyPaneTextField('title', {
                   label: "Game Title"
                 }),
                 PropertyPaneTextField('list', {
                   label: "Word List Name"
                 })
               ]
             }
           ]
         }
       ]
     };
   }
   ```

1. Stop serving, re-serve with `heft start --nobrowser`, and remove/re-add your web part to pick up the manifest changes.

1. Celebrate! 🎉 You now have a fully data-driven Wordle game!


If you run into any trouble or don't really want to do the steps above, you can just replace the entire contents of the **WordleWebPart.ts** file with the following:

<details>
<summary>:hedgehog: WordleWebPart.ts</summary>

```TypeScript
import { Version } from '@microsoft/sp-core-library';
import {
  type IPropertyPaneConfiguration,
  PropertyPaneTextField
} from '@microsoft/sp-property-pane';
import { BaseClientSideWebPart } from '@microsoft/sp-webpart-base';
import type { IReadonlyTheme } from '@microsoft/sp-component-base';

import styles from './WordleWebPart.module.scss';
import * as strings from 'WordleWebPartStrings';
import { escape } from '@microsoft/sp-lodash-subset';
import { IWordItem } from './IWordItem';
import { spfi, SPFx } from '@pnp/sp';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/items';

export interface IWordleWebPartProps {
  title: string;
  list: string;
}

export default class WordleWebPart extends BaseClientSideWebPart<IWordleWebPartProps> {

  // Game state - stored in memory (resets on page refresh)
  private guesses: string[] = [];
  private currentGuess: string = '';
  private maxGuesses: number = 6;
  private gameStatus: string = 'playing'; // 'playing', 'won', or 'lost'
  private targetWord: string = '';

  private words: IWordItem[] = [];
  private wordsLoaded: boolean = false;


  private renderGrid(): string {
    let gridHtml = '';

    for (let row = 0; row < this.maxGuesses; row++) {
      gridHtml += `<div class="${styles.row}">`;

      const guess = this.guesses[row] || '';
      const isCurrentRow = row === this.guesses.length;
      const displayGuess = isCurrentRow ? this.currentGuess : guess;

      for (let col = 0; col < 5; col++) {
        const letter = displayGuess[col] || '';
        const stateClass = guess ? this.getTileState(guess, col, this.targetWord) : (letter ? styles.filled : '');
        gridHtml += `<div class="${styles.tile} ${stateClass}">${escape(letter)}</div>`;
      }

      gridHtml += '</div>';
    }

    return gridHtml;
  }

  private getTileState(guess: string, index: number, targetWord: string): string {
    const letter = guess[index];
    
    if (letter === targetWord[index]) {
      return styles.correct;
    } else if (targetWord.indexOf(letter) >= 0) {
      return styles.present;
    } else {
      return styles.absent;
    }
  }

  private handleKeyDown = (event: KeyboardEvent): void => {
    // Don't do anything if game is over
    if (this.gameStatus !== 'playing') {
      return;
    }

    const key = event.key.toUpperCase();

    if (key === 'BACKSPACE') {
      // Remove last letter from current guess
      this.currentGuess = this.currentGuess.slice(0, -1);
      this.render();
    } else if (key === 'ENTER') {
      // Submit the guess (we'll implement this next)
      this.submitGuess();
    } else if (key.length === 1 && key >= 'A' && key <= 'Z') {
      // Add letter if we haven't reached 5 letters yet
      if (this.currentGuess.length < 5) {
        this.currentGuess += key;
        this.render();
      }
    }
  }
  private submitGuess(): void {
    // Must be exactly 5 letters
    if (this.currentGuess.length !== 5) {
      return;
    }

    // Add to guesses array
    this.guesses.push(this.currentGuess);

    // Check if they won
    if (this.currentGuess === this.targetWord.toUpperCase()) {
      this.gameStatus = 'won';
    }
    // Check if they lost (used all guesses)
    else if (this.guesses.length >= this.maxGuesses) {
      this.gameStatus = 'lost';
    }

    // Clear current guess for next round
    this.currentGuess = '';

    // Re-render to show the results
    this.render();
  }

  /**
* Gets the list of words from SharePoint
*
* @private
* @memberof WordleWebPart
*/
  private getWords = async (): Promise<void> => {
     const sp = spfi().using(SPFx(this.context));
     this.words = await sp.web.lists.getByTitle(this.properties.list).items.select('Title', 'WordCategory')();
     this.wordsLoaded = true;
     
     // Pick a random word for this game
     this.targetWord = this.getRandomWord();
     
     console.log("Words loaded:", this.words.length, "Target:", this.targetWord);
     this.render();
  }

  /**
   * Gets a random word from the loaded words
   */
  private getRandomWord(): string {
    if (this.words.length === 0) {
      return 'HELLO'; // Fallback if no words loaded
    }
    const randomIndex = Math.floor(Math.random() * this.words.length);
    return this.words[randomIndex].Title.toUpperCase();
  }

  protected onInit(): Promise<void> {
    document.addEventListener('keydown', this.handleKeyDown);

    // Load words from SharePoint
    this.getWords().catch((error) => console.error(error));

    return Promise.resolve();
  }

  protected onDispose(): void {
    document.removeEventListener('keydown', this.handleKeyDown);
  }

  public render(): void {
    if (!this.wordsLoaded) {
      this.context.statusRenderer.displayLoadingIndicator(this.domElement, 'words');
      return;
    }
    this.context.statusRenderer.clearLoadingIndicator(this.domElement);

    // Build the title with status emoji
    let titleText = escape(this.properties.title);
    if (this.gameStatus === 'won') {
      titleText += ' 🎉';
    } else if (this.gameStatus === 'lost') {
      titleText += ' 😢 (It was ' + escape(this.targetWord) + ')';
    }

    this.domElement.innerHTML = `
        <div class="${styles.wordle}">
          <div class="${styles.title}">
            ${titleText}
          </div>
          <div class="${styles.grid}">
            ${this.renderGrid()}
          </div>
        </div>`;
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
                PropertyPaneTextField('title', {
                  label: "Game Title"
                }),
                 PropertyPaneTextField('list', {
                  label: "Word List Name"
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


## :tada: All Done!
![Great Job!](assets/GreatJob.png)

You now have a fully data-driven Wordle game! 🎮 Each time you load the page, a random word is selected from your SharePoint list. Want a new word? Just refresh!

In the next lab, we'll add some configuration options like showing a category hint to customize the game experience.

# [Previous](../Lab08/README.md) | [Next](../Lab10/README.md)
