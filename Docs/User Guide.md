# VeriStand Communications Bus Template User Guide

The template provided in this repo is intended to be used for creating VeriStand custom devices that implement communication protocols. This guide aims to provide some theory on the template design details on how to use the template to create a fully functional custom device.

## Overview

The main goal of this template is to enable custom device authors to implement the configuration and messaging for a given protocol only once. The protocol and hardware configurations are implemented separately to more easily enable supporting multiple vendors or hardware devices in a new custom device without having to start everything from a blank custom device template.

Much of the overhead of creating custom devices is adapting LabVIEW code to correctly interface with VeriStand. The design of this template enables custom device authors to focus more on the code to implement a communication protocol and less on how VeriStand interacts with that code. The [Developer Guide](Developer%20Guide.md) provides more theory on the design of the template.

## Creating a New Custom Device

A utility for creating a new custom device from the template is located in this repo at `Source/Template Generator`. To generate a new custom device:

1. Open the VI at `Source/Template Generator/Template Tool/Clone Template Main.vi`.
2. Follow the instructions on the VI's front panel to set the custom device name and output LabVIEW project location.
3. Navigate to the **Destination Directory** provided in the VI and explore the custom device project.

After the tool completes, the new custom device will be usable from VeriStand.

## Theory of Operation

The most flexible way of creating extensible architectures in LabVIEW is by using LabVIEW classes and interfaces. To create class or interface overrides, LabVIEW requires a new VI of the same name, with the same connector pane to exist. This presents challenges to creating extensible and reusable code pieces in custom devices because VeriStand requires top-level custom device libraries to be saved as `.llb` files. These files have a flat structure, which means two files of the same name can not exist in these file types.

The newly generated custom device has a different file structure than one generated from the [Custom Device Wizard](https://github.com/ni/niveristand-custom-device-wizard). Instead of a single LabVIEW project in the `Source` directory, there are two projects:

- Custom Device Project
- Support Project

### Custom Device Project

The `Source/Custom Device/<MyName> Custom Device.lvproj` primarily exists for historical reasons. This project contains the code and build specs required for interacting with VeriStand. It builds the `.llb` files that VeriStand requires for execution.

This project has the same structure as a project generated from the wizard, but contains minimal logic for performing work in the custom device. Most of the code is a set of wrappers that call code implemented in packed project libraries built from the support project. These libraries can be found in the `Includes` virtual folder in the project.

### Support Project

The `Source/Custom Device Support/<MyName> Support.lvproj` is the project where almost all logic for the custom device's behavior will exist. This project contains the logic for interacting with hardware, encoding/decoding bus messages, System Explorer pages, and database management. It builds packed project libraries (PPLs) in the form of `Engine.lvlibp` and `System Explorer.lvlibp` files, which support hierarchical file structures. These PPLs enable the custom device author to utilize classes and interfaces to implement extensible architectures. This strategy also promotes consistency of experience across custom devices.

### Execution Unit

The fundamental piece of the engine for custom devices using this template is the **Execution Unit**.

## System Explorer

This template utilizes a dispatch pattern for retrieving pages and run-time menus to display in System Explorer. VI wrappers in the custom device project request page and run-time menu information from a **System Explorer Dispatcher** object and display them in System Explorer.

### Dispatcher

The custom device XML specifies `Page Wrapper.vi` as the VI to execute when an item is selected in the tree. This VI delegates to page implementations in `<MyName> System Explorer.lvlibp` from the support project. To add pages, a new **Page**, with new GUID, needs to be added to the custom device XML. The VI and GUID then must be added to `System Explorer Dispatcher.lvclass:Get Page.vi`.

Similarly, any run-time menus should also use the **System Explorer Dispatcher** to dynamically create right-click menus for a page. `RunTimeMenu Wrapper.vi` asks the dispatcher which menu to disply through `System Explorer Dispatcher.lvclass:Get Menu Item VI.vi` and what items to populate in that menu through `System Explorer Dispatcher:Get Menu Items.vi`.

**Note:** Dynamic buttons at the top of the System Explorer window can not be added through the `Page Wrapper.vi` mechanism, so any pages requiring those buttons will require a standard page to be created.

## ActionVIOnCompile

### Execution Unit

This is where they are created. Talk more about them in the support project section.

## Engine

### Separation of Protocol and Device

Maybe in same getting started as PPL stuff.

### Scripting API

### System Explorer

### Engine

#### G Interfaces

##### Execution Unit

##### Execution Unit Factory

#### Interface Implementation
