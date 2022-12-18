## Bonus exercise: Adding column formatting

> This step is optional, but it will make Chris Kent happy.

1. Using your browser, navigate the the **Powers** list that was provisioned when you deployed the solution.
1. Using the chevron next to the **Icons** column, select **Column settings**, then **Format this column**
   ![Context menu](assets/format-column.png)
1. In the **Format Icon column** pane, select **Advanced mode**
   ![Advanced mode](assets/formatpane.png)
1. Replace the formatting `JSON` with the following `JSON` and select **Save**:
   ![Icon formatting](assets/iconformatting.png)

   ```json
   {
      "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
      "elmType": "div",
      "style": {
        "flex-wrap": "wrap",
        "display": "flex"
      },
      "children": [
        {
          "forEach": "choiceIterator in @currentField",
          "elmType": "div",
          "style": {
            "display": "flex",
            "font-size": "24px",
            "align-items": "center",
            "white-space": "nowrap",
            "overflow": "hidden",
            "margin": "4px 4px 4px 4px"
          },
          "attributes": {
            "iconName": "[$choiceIterator]",
            "title": "[$choiceIterator]"
          }
        }
      ]
    }
   ```

1. Using the **Choose Column** drop-down, select **Colors** and replace the `JSON` with the following, then hit **Save**:
   ![Colors formatting](assets/colorsformatting.png)

   ```json
   {
      "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
      "elmType": "div",
      "style": {
        "flex-wrap": "wrap",
        "display": "flex"
      },
      "children": [
        {
          "forEach": "choiceIterator in @currentField",
          "elmType": "div",
          "style": {
            "box-sizing": "border-box",
            "padding": "4px 8px 5px 8px",
            "display": "flex",
            "border-radius": "16px",
            "height": "24px",
            "align-items": "center",
            "white-space": "nowrap",
            "overflow": "hidden",
            "margin": "4px 4px 4px 4px",
            "background-color": "[$choiceIterator]"
          },
          "children": [
            {
              "elmType": "span",
              "style": {
                "overflow": "hidden",
                "text-overflow": "ellipsis",
                "padding": "0 3px",
                "color": "white"
              },
              "txtContent": "[$choiceIterator]"
            }
          ]
        }
      ]
    }
   ```
