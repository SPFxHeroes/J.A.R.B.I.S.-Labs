# Lab 8: Provisioning Assets

Our web part needs to read data from a list to work.

Sure, we could give you the instructions to create the list and populate it, but that would be boring!

Plus, in the real world, you probably shouldn't expect users to go through complicated instructions just to use your web part!

In this lab, we're going to automatically provision a list when our web part is added to a site.

<details>
<summary><b>Legend</b></summary>

|Icon|Meaning|
|---|---|
|:rocket:|Exercise|
|:apple:|Mac specific instructions|
|:shield:|Admin mode required|
|:bulb:|Hot tip!|
|:books:|Resources|

</details>

<details>
<summary><b>Exercises</b></summary>

  1. [Add an elements.xml file for SharePoint definitions](#rocket-exercise-1-add-an-elementsxml-file-for-sharepoint-definitions)
  1. [Add a schema.xml file for defining list structure](#rocket-exercise-2-add-a-schemaxml-file-for-defining-list-structure)
  1. [Configure the SharePoint feature](#rocket-exercise-3-configure-the-sharepoint-feature)
  1. [Bundle and package the solution](#rocket-exercise-4-bundle-and-package-the-solution)
  1. [Deploy and provision to a site](#rocket-exercise-5-deploy-and-provision-to-a-site)
</details>

<details>
<summary><b>Starter Code</b></summary>

If you skipped the previous step, or just want to start here, you can find the code ready to go in the [Lab 08 Starter](https://github.com/SPFxHeroes/J.A.R.B.I.S./tree/Start-of-Lab-08) branch.

</details>

## :rocket: Exercise 1: Add an elements.xml file for SharePoint definitions

SPFx lets you take advantage of the Feature Framework to provision SharePoint assets with your package. You can provision Fields, Content types, List instances, and List instances with custom schema.

This is done by using a Feature. A feature is a container that includes one or more SharePoint items to provision. A feature is made up of a **Feature.xml** file and one or more element manifest files. You can even include files that handle upgrade previously provisioned assets. Wowee!

1. Using VS Code, at the root of your solution, create a folder called **sharepoint**.

1. Within the **sharepoint** folder, create an **assets** folder

1. In the **assets** folder, create a new file called **elements.xml**

1. Copy the following XML into the newly created file:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Elements xmlns="http://schemas.microsoft.com/sharepoint/">
        <Field ID="{B80C2FCD-A0B8-48A5-B225-37855393D899}" Name="Icon" DisplayName="Icon" Type="MultiChoice" Required="FALSE" FillInChoice="FALSE" Mult="TRUE" Group="SPFx Heroes">
            <CHOICES>
                <CHOICE>AirplaneSolid</CHOICE>
                <CHOICE>Arrivals</CHOICE>
                <CHOICE>AutoEnhanceOn</CHOICE>
                <CHOICE>Balloons</CHOICE>
                <CHOICE>BlowingSnow</CHOICE>
                <CHOICE>BugSolid</CHOICE>
                <CHOICE>BuildDefinition</CHOICE>
                <CHOICE>Bullseye</CHOICE>
                <CHOICE>BullseyeTarget</CHOICE>
                <CHOICE>Cake</CHOICE>
                <CHOICE>Calories</CHOICE>
                <CHOICE>Cat</CHOICE>
                <CHOICE>ClassroomLogo</CHOICE>
                <CHOICE>CityNext</CHOICE>
                <CHOICE>CityNext2</CHOICE>
                <CHOICE>ConstructionConeSolid</CHOICE>
                <CHOICE>DefenderTVM</CHOICE>
                <CHOICE>Diamond</CHOICE>
                <CHOICE>DeveloperTools</CHOICE>
                <CHOICE>Drop</CHOICE>
                <CHOICE>ExerciseTracker</CHOICE>
                <CHOICE>FangBody</CHOICE>
                <CHOICE>FlameSolid</CHOICE>
                <CHOICE>Flashlight</CHOICE>
                <CHOICE>Freezing</CHOICE>
                <CHOICE>Glasses</CHOICE>
                <CHOICE>HealthSolid</CHOICE>
                <CHOICE>HomeGroup</CHOICE>
                <CHOICE>AsteriskSolid</CHOICE>
                <CHOICE>Lamp</CHOICE>
                <CHOICE>LadybugSolid</CHOICE>
                <CHOICE>LightningBolt</CHOICE>
                <CHOICE>Manufacturing</CHOICE>
                <CHOICE>Medical</CHOICE>
                <CHOICE>MegaphoneSolid</CHOICE>
                <CHOICE>Microphone</CHOICE>
                <CHOICE>MessageFill</CHOICE>
                <CHOICE>NetworkTower</CHOICE>
                <CHOICE>PinnedSolid</CHOICE>
                <CHOICE>PinSolid12</CHOICE>
                <CHOICE>PermissionsSolid</CHOICE>
                <CHOICE>RainSnow</CHOICE>
                <CHOICE>Running</CHOICE>
                <CHOICE>Rocket</CHOICE>
                <CHOICE>Robot</CHOICE>
                <CHOICE>RibbonSolid</CHOICE>
                <CHOICE>SearchNearby</CHOICE>
                <CHOICE>SetAction</CHOICE>
                <CHOICE>Snowflake</CHOICE>
                <CHOICE>Squalls</CHOICE>
                <CHOICE>StreetsideSplitMinimize</CHOICE>
                <CHOICE>TestBeakerSolid</CHOICE>
                <CHOICE>Thunderstorms</CHOICE>
                <CHOICE>Umbrella</CHOICE>
                <CHOICE>Weights</CHOICE>
                <CHOICE>WifiWarning4</CHOICE>
                <CHOICE>WindDirection</CHOICE>
                <CHOICE>DefectSolid</CHOICE>
                <CHOICE>PlugSolid</CHOICE>
                <CHOICE>FitWidth</CHOICE>
                <CHOICE>Split</CHOICE>
            </CHOICES>
        </Field>
        <Field ID="{B89B5C5E-5485-4EEA-95BD-AD2842A19CA0}" Name="Colors" DisplayName="Colors" Type="MultiChoice" Required="FALSE" FillInChoice="FALSE" Mult="TRUE" Group="SPFx Heroes">
            <CHOICES>
                <CHOICE>Pink</CHOICE>
                <CHOICE>DeepPink</CHOICE>
                <CHOICE>DarkViolet</CHOICE>
                <CHOICE>DarkMagenta</CHOICE>
                <CHOICE>Indigo</CHOICE>
                <CHOICE>Salmon</CHOICE>
                <CHOICE>Red</CHOICE>
                <CHOICE>FireBrick</CHOICE>
                <CHOICE>DarkRed</CHOICE>
                <CHOICE>Orange</CHOICE>
                <CHOICE>DarkOrange</CHOICE>
                <CHOICE>Gold</CHOICE>
                <CHOICE>Yellow</CHOICE>
                <CHOICE>LawnGreen</CHOICE>
                <CHOICE>YellowGreen</CHOICE>
                <CHOICE>Green</CHOICE>
                <CHOICE>MediumAquaMarine</CHOICE>
                <CHOICE>Teal</CHOICE>
                <CHOICE>DarkTurquoise</CHOICE>
                <CHOICE>SteelBlue</CHOICE>
                <CHOICE>DeepSkyBlue</CHOICE>
                <CHOICE>DodgerBlue</CHOICE>
                <CHOICE>Navy</CHOICE>
                <CHOICE>SaddleBrown</CHOICE>
                <CHOICE>Maroon</CHOICE>
                <CHOICE>Ivory</CHOICE>
                <CHOICE>Silver</CHOICE>
                <CHOICE>DimGray</CHOICE>
                <CHOICE>Black</CHOICE>
            </CHOICES>
        </Field>
        <Field ID="{717A6C70-074C-4504-89B3-FEB55C77C709}" Name="Prefix" DisplayName="Prefix" Type="MultiChoice" Required="FALSE" FillInChoice="FALSE" Mult="TRUE" Group="SPFx Heroes">
            <CHOICES>
                <CHOICE>Defiant</CHOICE>
                <CHOICE>Tiny</CHOICE>
                <CHOICE>Little</CHOICE>
                <CHOICE>Big</CHOICE>
                <CHOICE>Colossal</CHOICE>
                <CHOICE>Giant</CHOICE>
                <CHOICE>Galactic</CHOICE>
                <CHOICE>Impervious</CHOICE>
                <CHOICE>Steel</CHOICE>
                <CHOICE>Ice</CHOICE>
                <CHOICE>Icy</CHOICE>
                <CHOICE>Water</CHOICE>
                <CHOICE>Fire</CHOICE>
                <CHOICE>Fiery</CHOICE>
                <CHOICE>Flame</CHOICE>
                <CHOICE>Electric</CHOICE>
                <CHOICE>Nuclear</CHOICE>
                <CHOICE>Fast</CHOICE>
                <CHOICE>Quick</CHOICE>
                <CHOICE>Speedy</CHOICE>
                <CHOICE>Nimble</CHOICE>
                <CHOICE>Aqua</CHOICE>
                <CHOICE>Shifty</CHOICE>
                <CHOICE>Sharp</CHOICE>
                <CHOICE>Slimy</CHOICE>
                <CHOICE>Robotic</CHOICE>
                <CHOICE>Slippery</CHOICE>
                <CHOICE>Gooey</CHOICE>
                <CHOICE>Stabby</CHOICE>
                <CHOICE>Magnetic</CHOICE>
                <CHOICE>Accurate</CHOICE>
                <CHOICE>Sneezy</CHOICE>
                <CHOICE>Electric</CHOICE>
                <CHOICE>Blue</CHOICE>
                <CHOICE>Green</CHOICE>
                <CHOICE>Red</CHOICE>
                <CHOICE>Purple</CHOICE>
                <CHOICE>Golden</CHOICE>
                <CHOICE>Pink</CHOICE>
                <CHOICE>Silver</CHOICE>
                <CHOICE>Scarlet</CHOICE>
                <CHOICE>Hypnotic</CHOICE>
                <CHOICE>Whispering</CHOICE>
                <CHOICE>Secret</CHOICE>
                <CHOICE>Doctor</CHOICE>
                <CHOICE>Captain</CHOICE>
                <CHOICE>General</CHOICE>
                <CHOICE>Lieutenant</CHOICE>
                <CHOICE>Admiral</CHOICE>
                <CHOICE>Cold</CHOICE>
                <CHOICE>Frozen</CHOICE>
                <CHOICE>Winter</CHOICE>
                <CHOICE>Burning</CHOICE>
                <CHOICE>Sizzling</CHOICE>
                <CHOICE>Hot</CHOICE>
                <CHOICE>Burnt</CHOICE>
                <CHOICE>Toasty</CHOICE>
                <CHOICE>Magnificent</CHOICE>
                <CHOICE>Infamous</CHOICE>
                <CHOICE>Spectacular</CHOICE>
                <CHOICE>Fantastic</CHOICE>
                <CHOICE>Mighty</CHOICE>
                <CHOICE>Super</CHOICE>
                <CHOICE>Amazing</CHOICE>
                <CHOICE>Incredible</CHOICE>
            </CHOICES>
        </Field>
        <Field ID="{B42A674A-9940-49D2-884F-A4F6378E6BAB}" Name="Main" DisplayName="Main" Type="MultiChoice" Required="FALSE" FillInChoice="FALSE" Mult="TRUE" Group="SPFx Heroes">
            <CHOICES>
                <CHOICE>Cheetah</CHOICE>
                <CHOICE>Wolf</CHOICE>
                <CHOICE>Falcon</CHOICE>
                <CHOICE>Fox</CHOICE>
                <CHOICE>Birdie</CHOICE>
                <CHOICE>Horse</CHOICE>
                <CHOICE>Lynx</CHOICE>
                <CHOICE>Rhino</CHOICE>
                <CHOICE>Hornet</CHOICE>
                <CHOICE>Spider</CHOICE>
                <CHOICE>Tiger</CHOICE>
                <CHOICE>Moth</CHOICE>
                <CHOICE>Owl</CHOICE>
                <CHOICE>Ant</CHOICE>
                <CHOICE>Penguin</CHOICE>
                <CHOICE>Cat</CHOICE>
                <CHOICE>Kitten</CHOICE>
                <CHOICE>Mantis</CHOICE>
                <CHOICE>Vulture</CHOICE>
                <CHOICE>Bear</CHOICE>
                <CHOICE>Bat</CHOICE>
                <CHOICE>Fish</CHOICE>
                <CHOICE>Eagle</CHOICE>
                <CHOICE>Scorpion</CHOICE>
                <CHOICE>Dolphin</CHOICE>
                <CHOICE>Badger</CHOICE>
                <CHOICE>Sparrow</CHOICE>
                <CHOICE>Raccoon</CHOICE>
                <CHOICE>Shark</CHOICE>
                <CHOICE>Crow</CHOICE>
                <CHOICE>Mastermind</CHOICE>
                <CHOICE>Marksman</CHOICE>
                <CHOICE>Huntress</CHOICE>
                <CHOICE>Hunter</CHOICE>
                <CHOICE>Ninja</CHOICE>
                <CHOICE>Smasher</CHOICE>
                <CHOICE>Flame</CHOICE>
                <CHOICE>Goo</CHOICE>
                <CHOICE>Duke</CHOICE>
                <CHOICE>Baron</CHOICE>
                <CHOICE>General</CHOICE>
                <CHOICE>Shadow</CHOICE>
                <CHOICE>Magician</CHOICE>
                <CHOICE>Wizard</CHOICE>
                <CHOICE>Amazon</CHOICE>
                <CHOICE>Axeman</CHOICE>
                <CHOICE>Agent</CHOICE>
                <CHOICE>Assassin</CHOICE>
                <CHOICE>Gladiator</CHOICE>
                <CHOICE>Hammer</CHOICE>
                <CHOICE>Watcher</CHOICE>
                <CHOICE>Angel</CHOICE>
                <CHOICE>Sentinel</CHOICE>
                <CHOICE>Soldier</CHOICE>
                <CHOICE>Sage</CHOICE>
                <CHOICE>Master</CHOICE>
                <CHOICE>Prodigy</CHOICE>
                <CHOICE>Genius</CHOICE>
                <CHOICE>Conductor</CHOICE>
                <CHOICE>Accountant</CHOICE>
                <CHOICE>Doctor</CHOICE>
                <CHOICE>Warden</CHOICE>
                <CHOICE>Shield</CHOICE>
                <CHOICE>Gunner</CHOICE>
                <CHOICE>Warrior</CHOICE>
                <CHOICE>Sneezer</CHOICE>
                <CHOICE>Robot</CHOICE>
                <CHOICE>Veterinarian</CHOICE>
                <CHOICE>Zookeeper</CHOICE>
                <CHOICE>Electrician</CHOICE>
                <CHOICE>Plumber</CHOICE>
                <CHOICE>Slime</CHOICE>
                <CHOICE>Zap</CHOICE>
                <CHOICE>Shifter</CHOICE>
                <CHOICE>Fist</CHOICE>
                <CHOICE>Taffy</CHOICE>
                <CHOICE>Arrow</CHOICE>
                <CHOICE>Eye</CHOICE>
                <CHOICE>Icecube</CHOICE>
                <CHOICE>Rocket</CHOICE>
                <CHOICE>Speedster</CHOICE>
                <CHOICE>Locomotive</CHOICE>
                <CHOICE>Train</CHOICE>
                <CHOICE>Snowman</CHOICE>
                <CHOICE>Freeze</CHOICE>
                <CHOICE>Toast</CHOICE>
                <CHOICE>Ashes</CHOICE>
                <CHOICE>Fire</CHOICE>
                <CHOICE>Burn</CHOICE>
            </CHOICES>
        </Field>
        <ContentType ID="0x0100CA39ECEC4E224C4CACA336D3CE5D8BFD" Name="Powers" Group="SPFx Content Types" Description="SPFx Heroes">
            <FieldRefs>
                <FieldRef ID="{B80C2FCD-A0B8-48A5-B225-37855393D899}" />
                <FieldRef ID="{B89B5C5E-5485-4EEA-95BD-AD2842A19CA0}" />
                <FieldRef ID="{717A6C70-074C-4504-89B3-FEB55C77C709}" />
                <FieldRef ID="{B42A674A-9940-49D2-884F-A4F6378E6BAB}" />
            </FieldRefs>
        </ContentType>
        <ListInstance CustomSchema="schema.xml" FeatureId="00bfea71-de22-43b2-a848-c05709900100" Title="Powers" Description="Contains list of compatible powers" TemplateType="100" Url="Lists/Powers">
            <Data>
                <Rows>
                    <Row>
                        <Field Name="Title">Animal Friends</Field>
                        <Field Name="Icon">;#Cat;#BugSolid;#FangBody;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Doctor;#Magnificent;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#</Field>
                        <Field Name="Main">;#Cheetah;#Wolf;#Falcon;#Fox;#Birdie;#Horse;#Lynx;#Rhino;#Hornet;#Spider;#Tiger;#Moth;#Owl;#Ant;#Penguin;#Cat;#Kitten;#Mantis;#Vulture;#Bear;#Bat;#Fish;#Eagle;#Scorpion;#Dolphin;#Badger;#Sparrow;#Raccoon;#Shark;#Crow;#Mastermind;#Duke;#Wizard;#Magician;#Warden;#Zookeeper;#Veterinarian;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Archery</Field>
                        <Field Name="Icon">;#Bullseye;#BullseyeTarget;#ClassroomLogo;#RibbonSolid;#WindDirection;#</Field>
                        <Field Name="Colors">;#LawnGreen;#YellowGreen;#Green;#SaddleBrown;#DimGray;#Black;#</Field>
                        <Field Name="Prefix">;#Defiant;#Fast;#Quick;#Speedy;#Nimble;#Sharp;#Accurate;#Green;#Silver;#Whispering;#Doctor;#Captain;#General;#Lieutenant;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Fox;#Hornet;#Mantis;#Sparrow;#Marksman;#Huntress;#Hunter;#Duke;#Crow;#Baron;#Shadow;#Amazon;#Assassin;#Watcher;#Angel;#Sentinel;#Warrior;#Arrow;#Eye;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Bulletproof</Field>
                        <Field Name="Icon">;#DefenderTVM;#Umbrella;#HealthSolid;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Defiant;#Impervious;#Steel;#Captain;#General;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Horse;#Rhino;#Mantis;#Bear;#Scorpion;#Huntress;#Hunter;#Duke;#Baron;#General;#Assassin;#Sentinel;#Soldier;#Master;#Shield;#Gunner;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Electricity</Field>
                        <Field Name="Icon">;#Flashlight;#HealthSolid;#Lamp;#SetAction;#DefectSolid;#PlugSolid;#LightningBolt;#</Field>
                        <Field Name="Colors">;#Yellow;#Gold;#Orange;#DarkOrange;#LawnGreen;#Ivory;#</Field>
                        <Field Name="Prefix">;#Electric;#Robotic;#Doctor;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Sentinel;#Conductor;#Robot;#Electrician;#Zap;#Ashes;#Toast;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Fire</Field>
                        <Field Name="Icon">;#Calories;#FlameSolid;#</Field>
                        <Field Name="Colors">;#Red;#Orange;#DarkOrange;#Maroon;#FireBrick;#</Field>
                        <Field Name="Prefix">;#Fire;#Fiery;#Flame;#Red;#Scarlet;#Captain;#Doctor;#General;#Lieutenant;#Burning;#Sizzling;#Hot;#Burnt;#Toasty;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Falcon;#Fox;#Birdie;#Rhino;#Tiger;#Cat;#Kitten;#Bear;#Scorpion;#Flame;#Duke;#Baron;#Wizard;#Agent;#Warrior;#Toast;#Ashes;#Fire;#Burn;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Flight</Field>
                        <Field Name="Icon">;#AirplaneSolid;#Arrivals;#Balloons;#</Field>
                        <Field Name="Colors">;#DodgerBlue;#DeepSkyBlue;#SteelBlue;#Navy;#DarkTurquoise;#Teal;#Indigo;#DarkMagenta;#DarkViolet;#MediumAquaMarine;#</Field>
                        <Field Name="Prefix">;#Galactic;#Blue;#Silver;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Falcon;#Birdie;#Hornet;#Moth;#Owl;#Vulture;#Bat;#Eagle;#Sparrow;#Crow;#Duke;#Angel;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Gadget Limbs</Field>
                        <Field Name="Icon">;#ConstructionConeSolid;#BuildDefinition;#Robot;#Manufacturing;#DeveloperTools;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#</Field>
                        <Field Name="Prefix">;#Steel;#Electric;#Nuclear;#Nimble;#Robotic;#Doctor;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Rhino;#Mantis;#Vulture;#Scorpion;#Badger;#Raccoon;#Mastermind;#Smasher;#Agent;#Hammer;#Conductor;#Doctor;#Robot;#Fist;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Guns</Field>
                        <Field Name="Icon">;#AsteriskSolid;#</Field>
                        <Field Name="Colors">;#Black;#DimGray;#Silver;#</Field>
                        <Field Name="Prefix">;#Defiant;#Steel;#Nimble;#Quick;#Accurate;#Silver;#Captain;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Scorpion;#Marksman;#Huntress;#Hunter;#Duke;#Baron;#General;#Shadow;#Assassin;#Sentinel;#Soldier;#Master;#Prodigy;#Gunner;#Warrior;#Eye;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Ice</Field>
                        <Field Name="Icon">;#BlowingSnow;#AutoEnhanceOn;#Squalls;#Snowflake;#RainSnow;#Freezing;#</Field>
                        <Field Name="Colors">;#Navy;#DodgerBlue;#DeepSkyBlue;#SteelBlue;#DarkTurquoise;#Silver;#DimGray;#Indigo;#</Field>
                        <Field Name="Prefix">;#Steel;#Ice;#Icy;#Blue;#Silver;#Doctor;#Admiral;#Cold;#Frozen;#Winter;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Bear;#Duke;#Wizard;#Assassin;#Angel;#Eye;#Icecube;#Penguin;#Snowman;#Freeze;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Knives</Field>
                        <Field Name="Icon">;#PinnedSolid;#PinSolid12;#</Field>
                        <Field Name="Colors">;#Silver;#DimGray;#Black;#Indigo;#DarkMagenta;#</Field>
                        <Field Name="Prefix">;#Defiant;#Impervious;#Steel;#Quick;#Nimble;#Sharp;#Stabby;#Silver;#Scarlet;#Doctor;#Captain;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Wolf;#Falcon;#Fox;#Lynx;#Spider;#Tiger;#Cat;#Kitten;#Scorpion;#Ninja;#Huntress;#Hunter;#Duke;#Baron;#Shadow;#Agent;#Assassin;#Sentinel;#Warden;#Shield;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Laser Eyes</Field>
                        <Field Name="Icon">;#RibbonSolid;#Glasses;#</Field>
                        <Field Name="Colors">;#Red;#Salmon;#DarkOrange;#Orange;#Maroon;#</Field>
                        <Field Name="Prefix">;#Impervious;#Fiery;#Shifty;#Red;#Scarlet;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Falcon;#Hornet;#Mantis;#Scorpion;#Duke;#Wizard;#Magician;#Watcher;#Warrior;#Zap;#Eye;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Magnetic</Field>
                        <Field Name="Icon">;#WifiWarning4;#StreetsideSplitMinimize;#HomeGroup;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Defiant;#Steel;#Magnetic;#Purple;#Secret;#Doctor;#Captain;#Lieutenant;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Mastermind;#Duke;#Baron;#General;#Wizard;#Sentinel;#Master;#Conductor;#Doctor;#Shifter;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Martial Arts</Field>
                        <Field Name="Icon">;#Running;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#</Field>
                        <Field Name="Prefix">;#Defiant;#Impervious;#Fast;#Nimble;#Blue;#Green;#Red;#Purple;#Golden;#Pink;#Silver;#Scarlet;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Cheetah;#Wolf;#Falcon;#Fox;#Spider;#Tiger;#Cat;#Kitten;#Mantis;#Scorpion;#Badger;#Crow;#Ninja;#Shadow;#Assassin;#Agent;#Sentinel;#Soldier;#Shield;#Warrior;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Mind Control</Field>
                        <Field Name="Icon">;#AutoEnhanceOn;#NetworkTower;#WifiWarning4;#PermissionsSolid;#</Field>
                        <Field Name="Colors">;#DeepPink;#Pink;#DarkViolet;#DarkMagenta;#Salmon;#</Field>
                        <Field Name="Prefix">;#Purple;#Doctor;#Admiral;#Hypnotic;#Whispering;#Secret;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Spider;#Owl;#Dolphin;#Mastermind;#Duke;#Baron;#General;#Magician;#Wizard;#Sentinel;#Sage;#Master;#Prodigy;#Genius;#Doctor;#Eye;#Accountant;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Regeneration</Field>
                        <Field Name="Icon">;#Drop;#HealthSolid;#Medical;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Nuclear;#Blue;#Green;#Red;#Purple;#Golden;#Pink;#Silver;#Scarlet;#Doctor;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Owl;#Mantis;#Eagle;#Magician;#Wizard;#Angel;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Shape Shifting</Field>
                        <Field Name="Icon">;#SearchNearby;#</Field>
                        <Field Name="Colors">;#Navy;#DodgerBlue;#DeepSkyBlue;#Indigo;#Black;#SteelBlue;#</Field>
                        <Field Name="Prefix">;#Nimble;#Shifty;#Captain;#Lieutenant;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Lynx;#Spider;#Goo;#Duke;#Magician;#Master;#Shifter;#Taffy;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Shrinking</Field>
                        <Field Name="Icon">;#Cake;#BugSolid;#HomeGroup;#SearchNearby;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Tiny;#Little;#Nuclear;#Secret;#Doctor;#Captain;#General;#Lieutenant;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Hornet;#Spider;#Moth;#Ant;#Kitten;#Sparrow;#Ninja;#Assassin;#Shadow;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Slime</Field>
                        <Field Name="Icon">;#CircleFill;#Drop;#CircleHalfFull;#</Field>
                        <Field Name="Colors">;#LawnGreen;#DeepPink;#Pink;#Salmon;#MediumAquaMarine;#DeepSkyBlue;#</Field>
                        <Field Name="Prefix">;#Water;#Nuclear;#Nimble;#Shifty;#Slimy;#Slippery;#Gooey;#Green;#Pink;#Secret;#General;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Moth;#Mantis;#Fish;#Goo;#Ninja;#Baron;#Slime;#Taffy;#Shifter;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Sonic Sneeze</Field>
                        <Field Name="Icon">;#Squalls;#TestBeakerSolid;#</Field>
                        <Field Name="Colors">;#LawnGreen;#Green;#MediumAquaMarine;#YellowGreen;#Red;#</Field>
                        <Field Name="Prefix">;#Colossal;#Giant;#Slimy;#Gooey;#Sneezy;#Green;#General;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Goo;#Sneezer;#Slime;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Stretchy</Field>
                        <Field Name="Icon">;#FitWidth;#Split;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Nimble;#Shifty;#Slippery;#Gooey;#Silver;#Doctor;#Blue;#Green;#Red;#Purple;#Golden;#Pink;#Scarlet;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Lynx;#Spider;#Cat;#Kitten;#Ninja;#Goo;#Shadow;#Taffy;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Super Speed</Field>
                        <Field Name="Icon">;#SetAction;#Rocket;#Running;#ExerciseTracker;#LightningBolt</Field>
                        <Field Name="Colors">;#Red;#DarkOrange;#Orange;#DeepPink;#Pink;#FireBrick;#</Field>
                        <Field Name="Prefix">;#Fiery;#Fast;#Quick;#Speedy;#Nimble;#Red;#Pink;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Cheetah;#Wolf;#Falcon;#Fox;#Baron;#Angel;#Zap;#Rocket;#Speedster;#Locomotive;#Train;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Super Strength</Field>
                        <Field Name="Icon">;#DefenderTVM;#Weights;#Diamond;#ConstructionConeSolid;#</Field>
                        <Field Name="Colors">;#Pink;#DeepPink;#DarkViolet;#DarkMagenta;#Indigo;#Salmon;#Red;#FireBrick;#DarkRed;#Orange;#DarkOrange;#Gold;#LawnGreen;#Yellow;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#SaddleBrown;#Maroon;#Ivory;#DimGray;#Silver;#Black;#</Field>
                        <Field Name="Prefix">;#Defiant;#Big;#Colossal;#Giant;#Galactic;#Impervious;#Steel;#Nuclear;#Blue;#Green;#Red;#Purple;#Golden;#Pink;#Silver;#Scarlet;#Lieutenant;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Rhino;#Tiger;#Bear;#Smasher;#General;#Gladiator;#Hammer;#Sentinel;#Soldier;#Warden;#Shield;#Warrior;#Fist;#</Field>
                    </Row>
                    <Row>
                        <Field Name="Title">Talk to Fish</Field>
                        <Field Name="Icon">;#MessageFill;#MegaphoneSolid;#Microphone;#</Field>
                        <Field Name="Colors">;#LawnGreen;#YellowGreen;#Green;#MediumAquaMarine;#Teal;#DarkTurquoise;#SteelBlue;#DeepSkyBlue;#DodgerBlue;#Navy;#Silver;#DimGray;#Indigo;#</Field>
                        <Field Name="Prefix">;#Water;#Aqua;#Slippery;#Blue;#Green;#Whispering;#Hypnotic;#Secret;#Doctor;#Admiral;#Incredible;#Amazing;#Super;#Mighty;#Fantastic;#Spectacular;#Infamous;#Magnificent;#</Field>
                        <Field Name="Main">;#Penguin;#Fish;#Dolphin;#Shark;#Mastermind;#Duke;#Baron;#Magician;#Wizard;#Prodigy;#Genius;#Plumber;#</Field>
                    </Row>
                </Rows>
            </Data>
        </ListInstance>
    </Elements>
    ```

There are a few things to take note of from this XML:

- We're provisioning 4 fields to the site, a content type and a list instance with custom schema.
- Definitions use standard Feature Framework schema, which is "well known" to SharePoint developers.
- Custom fields are being referenced in the introduced content type.
- We use the `CustomSchema` attribute in the `ListInstance` element to define a provisioning-time `schema.xml` file for the list. This way the list is still based on the out-of-the-box list template (normal custom list template `100` in this case), but we can define an alternative provisioning definition during initial provisioning.
- When provisioning list instances using Features, you must provide the `ID` of the Feature associated with the particular list definition. Using the `FeatureId` attribute you're supposed to provide the `ID` of the Feature, which contains the List Definition. As an example: if you’re provisioning an instance of a custom list the `FeatureId` attribute should be set to `{00bfea71-de22-43b2-a848-c05709900100}`.
- In addition to structure, we're also provisioning default list items. This makes our list immediately usable and is generally a good way to get people started when using this approach.

#### :books: Resources
- [Provision SharePoint assets (SPFx)](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/toolchain/provision-sharepoint-assets)
- [Using Features in SharePoint Foundation](https://learn.microsoft.com/previous-versions/office/developer/sharepoint-2010/ms460318(v=office.14)?redirectedfrom=MSDN).
- [Element manifest file](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/toolchain/provision-sharepoint-assets#element-manifest-file)

## :rocket: Exercise 2: Add a schema.xml file for defining list structure

In the previous step, we referenced the **schema.xml** file in the `CustomSchema` attribute of the `ListInstance` element (line 271), so we need to include that file in our package.

Any supported files that accompany the element manifest (like the **schema.xml** file we're about to create) are called element files.

1. Create a new file called **schema.xml** in the **sharepoint\assets** folder.

1. Copy the following XML into the new **schema.xml** file:

    ```xml
    <List xmlns:ows="Microsoft SharePoint" Title="Basic List" EnableContentTypes="TRUE" FolderCreation="FALSE" Direction="$Resources:Direction;" Url="Lists/Basic List" BaseType="0"
    xmlns="http://schemas.microsoft.com/sharepoint/">
        <MetaData>
            <ContentTypes>
                <ContentTypeRef ID="0x0100CA39ECEC4E224C4CACA336D3CE5D8BFD" />
            </ContentTypes>
            <Fields></Fields>
            <Views>
                <View BaseViewID="1" Type="HTML" WebPartZoneID="Main" DisplayName="$Resources:core,objectiv_schema_mwsidcamlidC24;" DefaultView="TRUE" MobileView="TRUE" MobileDefaultView="TRUE" SetupPath="pages\viewpage.aspx" ImageUrl="/_layouts/images/generic.png" Url="AllItems.aspx">
                    <XslLink Default="TRUE">main.xsl</XslLink>
                    <JSLink>clienttemplates.js</JSLink>
                    <RowLimit Paged="TRUE">30</RowLimit>
                    <Toolbar Type="Standard" />
                    <ViewFields>
                        <FieldRef Name="Title"></FieldRef>
                        <FieldRef Name="Icon"></FieldRef>
                        <FieldRef Name="Colors"></FieldRef>
                        <FieldRef Name="Prefix"></FieldRef>
                        <FieldRef Name="Main"></FieldRef>
                    </ViewFields>
                    <Query>
                        <OrderBy>
                            <FieldRef Name="ID" />
                        </OrderBy>
                    </Query>
                </View>
            </Views>
            <Forms>
                <Form Type="DisplayForm" Url="DispForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />
                <Form Type="EditForm" Url="EditForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />
                <Form Type="NewForm" Url="NewForm.aspx" SetupPath="pages\form.aspx" WebPartZoneID="Main" />
            </Forms>
        </MetaData>
    </List>
    ```

#### :books: Resources
- [Element files](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/toolchain/provision-sharepoint-assets#element-file)


## :rocket: Exercise 3: Configure the SharePoint feature

At this point, we've created the files for provisioning SharePoint assets using the Feature schema from the solution when it's deployed. The next step is to include them in the SharePoint package ***.sppkg** file that will be created when we package our solution up.

To include the files, we must define the feature configuration in our project. We do this in the **config** > **package-solution.json** file. This file contains the metadata used by the build task (`gulp package-solution`) and determines the contents of the generated ***sppkg** file.

1. Open **package-solution.json** from the **config** folder.

1. In the **features** node, you'll see there's already a feature with a title of `jarbis Feature` and we'll be adding our provisioning files as assets to this feature. Add a comma after `"version": "1.0.0.0"` then paste the following"

    ```json
     "assets": {
        "elementManifests": [
          "elements.xml"
        ],
        "elementFiles":[
          "schema.xml"
        ]
      }
    ```

#### :books: Resources
- [Configure the SharePoint feature in SPFx](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/toolchain/provision-sharepoint-assets#configure-the-sharepoint-feature)


## :rocket: Exercise 4: Bundle and package the solution

Now you're ready to deploy the solution to SharePoint. Because we're provisioning assets directly to the SharePoint sites when the solution is installed, you can't test the provisioning capability in the Workbench.

We'll be making a non-production build just to verify our provisioning is correct (and to make the list and its content available while we're developing).

1. If your `gulp serve` is still running in the terminal, hit <kbd>CTRL</kbd>-<kbd>C</kbd> to stop it.

1. In the terminal in VS Code, execute the following command to bundle your client-side solution that contains the web part (combine and minimize your final javascript) to get the basic structure ready for packaging:
   ```bash
   gulp bundle
   ```

1. Once complete, execute the following command to create the solution package:
   ```bash
   gulp package-solution
   ```
   > :bulb: The command creates the **jarbis.sppkg** package in the **sharepoint/solution** folder.

   > :warning: You will see a warning message indicating that the solution is not a production build, and another one indicating that Feature.xml elements are not automatically applied unless the solution is explicitly installed on a site; both warnings are expected.

#### :books: Resources
- [Package solutions](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/basics/notes-on-solution-packaging)


## :rocket: Exercise 5: Deploy and provision to a site

Now that we've got a package file, we need to deploy it to our app catalog to make it available to be added to our site.

> :warning: We are assuming you are using a development tenant and so deploying a half-baked solution to the primary app catalog is fine. Don't do this on production! If you must use a production tenant, then you should be working in a Site Collection App Catalog. Setting this up is beyond the scope of this lab, but more details can be found in the links below.

1. In the browser, navigate to your tenant's app catalog by heading to the SharePoint Admin center (`https://YOURTENANT-admin.sharepoint.com`) - where `YOURTENANT` is your Microsoft 365 developer tenant name.

1. Using the navigation panel on the left, choose **More features** then click the **Open** button under Apps.

   ![SharePoint admin center](assets/admincenter.png)

1. Find the **jarbis.sppkg** package in the **sharepoint/solution** using File Explorer (Tip: right-click on the file from within VSCode and select **Reveal in File Explorer**

   ![Reveal in File Explorer](assets/reveal.png)

1. Drag and drop **jarbis.sppkg** from **File Explorer** onto the **App Catalog** or select **Upload** from the **Manage apps** page and select the **jarbis.sppkg** file from your desktop.

   ![App Catalog](assets/appcatalog.png)  

1. When prompted to **Enable app**, select **Only enable this app**; this will ensure that we only deploy the solution to our test site.

   ![Enable app](assets/addapp.png)

   > :bulb: Notice that the **Enable app** pane indicates **This app gets data from: https://localhost:4321/dist**; this is because the solution that we just bundled is _not_ intended for production and will only run while you debug the web part. We'll fix this in later labs, when we deploy the production web part.

1. Return to your workbench and select the **Settings** gear icon, followed by **Site contents** to view the site content where your workbench is located.

   ![Settings > Site contents](assets/settings-sitecontents.png)  

1. From the **Site contents** page, select **New**, followed by **App**.

   ![New > App](assets/sitecontents.png)  

1. From the **My apps** page, under **Apps you can add**, find **jarbis-client-side-solution** and select **Add**.

   ![Add jarbis-client-side-solution](assets/addjarbis.png)  

1. Once the solution has been added, you'll see a notification bar at the top of the page. Select the link to **Site contents page** to view the list that was deployed.

   ![Added successfully](assets/success-notification.png)

1. From the **Site contents** page, look for a list called **Powers** and select it

   ![Site contents with new Powers list](assets/sitecontents-powers.png)  

1. You'll see a list with several records. This is good!!!

   ![I've got the Powers!](assets/powers.png)

#### :books: Resources
- [Take a closer look](https://zoomquilt.org/)


## :tada: All Done!
![Great Job!](assets/GreatJob.png)

If you're up to it, there is also a [bonus lab](BONUS.md) to make that list a lot prettier! It's in no way necessary, but it sure is fun!

In our next lab, we'll wire up the web part to read list items from the list!

# [Previous](../Lab07/README.md) | [Next](../Lab09/README.md)