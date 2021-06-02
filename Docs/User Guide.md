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

## Source Layout

Uses PPLs so you don't have to do all the VeriStand stuff. (Maybe put this in a getting started/overview section?)

### Custom Device Project

Custom Device is VeriStand interface shim work.

#### System Explorer

##### Page Wrapper

#### ActionVIOnCompile

##### Execution Unit

This is where they are created. Talk more about them in the support project section.

#### Engine

### Support Project

Custom Device Support is where most things happen.

#### Separation of Protocol and Device

Maybe in same getting started as PPL stuff.

#### Scripting API

#### System Explorer

#### Engine

##### G Interfaces

###### Execution Unit

###### Execution Unit Factory

##### Interface Implementation
