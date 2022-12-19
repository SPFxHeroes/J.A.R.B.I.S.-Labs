# Lab 13: Publishing Solution

So far, we have been testing our web part using the workbench and bundling the solution in debug mode.

In this lab, we'll show you how to prepare your solution for release to production, and how to deploy and upgrade your solution.

## Exercise 1

1. Let's start by changing the solution's metadata; open the **package-solution.json** under the **config** folder.
1. In the `solution` node, change the `name` property to `J.A.R.B.I.S. Heroes`.
   > In your solution name, don't use words matching features in Microsoft Teams or SharePoint, such as **Chat, Contacts, Calendar, Calls, Files, Meeting, Activity, Teams, Apps, Help, SharePoint, List, Page**, etc. as these names could be confused with the standard functionality in Teams and SharePoint.
   >
   > If your solution is named after a common word, such as **Orders**, you should include your company name as well to clearly differentiate it from other solutions, for example, **Contoso Orders**.
   >
   > Luckily, J.A.R.B.I.S. is not common :-)
1. In the `metadata` node, change the `shortDescription`'s `default` to `Contains the J.A.R.B.I.S. hero generator web part and the Powers list`

   >  The description should always clearly describe what different components (web parts, application customizers, etc.) are included in the package to manage user expectations and help them understand the impact of using your application.
   >
   > The `shortDescription` is what will be displayed on the **Add an app** page.
1. Change the `longDescription` to `Contains the following:<ul><li><strong>J.A.R.B.I.S.</strong> <em>(Just a Rather Basic Instructional Solution)</em> hero generator web part</li><li>The <strong>Powers</strong> list, which contains a list of compatible powers, colors, icons, and descriptions used when generating a new super hero.</li></ul>`
   > The `longDescription` will be displayed on your app's **About** page. Unlike the `shortDescription`, you can use `HTML` here.
