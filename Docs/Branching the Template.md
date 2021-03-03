# Creating a new instance of the template

There are two ways to create a new instance of the template. You can use the automated VI that does all the work for you, or you can create a new instance using a step-by-step method. The automated method is fastest and easiest. The step-by-step method gives you full control and is useful if you need to understand any part of the process. The results are nearly identical.

# Creating a new instance of the template automatically

These steps will create a completely new template that will work side-by-side with the existing template.

1. Open `Template Generator\Template Tool\Clone Template Main.vi`.
1. Run the VI.
1. Enter a **Custom Device Name**.
1. Choose a **Destination Directory** and press the Start button.

The progress bar, along with messages, will keep you updated as the process proceeds.

Upon completion you will find a folder with your Custom Device Name in the Destination Directory location.

This tool also copies all files needed for your new Custom Device to show up in the VeriStand System Explorer. These files will be located in a folder with your Custom Device Name located in your public documents directory under National Instruments\NI VeriStand [YEAR]\Custom Devices. For example if your new custom device was named My New Comms for VeriStand 2020, you will find `C:\Users\Public\Documents\National Instruments\NI VeriStand 2020\Custom Devices\My New Comms`.


# Creating a new instance of the template step-by-step

These steps can be followed to create a new instance of the template. At the end of each major subsection the custom device should remain usable, although it won't work side-by-side with the template until the end.

Committing your changes after each major subsection is recommended, to allow you to partially restart if mistakes are made.

It is recommended that you close LabVIEW after each section involving renaming items. Issues with LabVIEW not handling renames properly have been observed after it has been open for multiple operations.

### Create a base branch for new development

The following will create a new branch for custom device development, without actually creating any new commits. This lets you use pull requests into the branch for all development.

```
git checkout main
git pull
git checkout -b xnet
git push -u origin xnet
```

You will then want to create a new development branch for the remaining work, which can be pull-requested/merged into the previously created branch.

## Change GUIDs

1. Open `Source\Utility\Replace All GUIDs.vi`, and select `Source\Custom Device\Shared\Constants.vi`, `Source\Custom Device Support\Scripting\Constants.vi` and `Source\Custom Device\Custom Device Communication Bus Template.xml`. Run the VI.

## Update installation directory

1. Open the custom device XML file in a text editor and replace all instances of `Custom Devices\Communication Bus Template` with `Custom Devices\Desired Install Location`.
1. Open `Source\Custom Device Support\Utility\Copy PPLs to NI VeriStand dir.vi` and change the `CD Name` constant to the desired install location.
1. Open `Source\Custom Device\Utility\Copy .LLB to NI VeriStand dir.vi` and change the `CD Name` constant to the desired install location.
1. Open `Source\Custom Device\Communication Bus Template Custom Device.lvproj` and edit the destination directory for all build specifications to be `Built\Desired Install Location` rather than `Built\Communication Bus Template`. <!-- Ouch. -->
1. Open the `build.toml` file in a text editor and update the `install_destination` for the custom device package; i.e. replace `Custom Devices\Communication Bus Template` with `Custom Devices\Desired Install Location`.

## Update custom device name

1. Open the custom device XML file in a text editor.
1. Replace the name in the `<AddMenu>` tags with the desired name, e.g. replace `National Instruments::Communication Bus Template` with `National Instruments::XNET`.
1. Find the page corresponding to the `<MainPageGUID>` and change the name to the desired name. This is the name used when creating a new instance of the custom device.

## Rename packed project libraries

1. Open `Source\Custom Device Support\Communication Bus Template Support.lvproj` and build both PPL build specifications for Windows.
1. Modify the "Target Filename" for all PPL build specifications to be the desired name.
1. Save the project.
1. Open `Source\Custom Device\Communication Bus Template Custom Device.lvproj`.
1. Right click `Includes/Communication Bus System Explorer.lvlibp` and select `Replace with...`. Select the new System Explorer PPL.
1. Repeat the above step for the Engine PPL (`Includes/Communication Bus Engine.lvlibp`).
1. Move the PPLs in the project back into the `Includes` directory, since LabVIEW moved them out.
1. Save all changes.
1. Open the custom device XML file in a text editor and replace all references to `Communication Bus Engine.lvlibp` with the new name.
1. Delete the old PPLs from the `Includes` directory.

<!-- I had to manually relink Initialize.vi... not sure why. If you reproduce this, we should investigate more. -->

## Rename custom device libraries

1. Open `Source\Custom Device\Communication Bus Template Custom Device.lvproj`.
1. Right click `Communication Bus Template System Explorer.lvlib` and select "Rename...". Rename the item to the desired name.
1. Repeat the above step for `Communication Bus Template Shared.lvlib` and `Communication Bus Template Engine.lvlib`.
1. Save all changes.

## Update package names

1. Open `control_custom_device` in a text editor and update (at least) the `Package`, `Description`, and `XB-DisplayName` properties.
1. Open `control_scripting` in a text editor and update (at least) the `Package`, `Description`, `XB-DisplayName`, and `Recommends` properties.
