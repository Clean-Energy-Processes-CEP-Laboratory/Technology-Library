# :one: Functional Specification

This document contains an overview of the functionality required and expected of the library.

#### Table of contents

* [Overview](#overview)
  * [Revieweres](#reviewers)
  * [Modification history](#modification-history)
* [Risks and assumptions](#risks-and-assumptions)
* [Use cases](#use-cases)
  * [Open-access repository](#green_book-open-access-repository)
  * [Component-determination interface](#crystal_ball-component-determination-interface)
  * [Plug-and-play modules](#electric_plug-plug-and-play-modules)
* [Requirements](#requirements)
  * [Interface requirements](#interface-requirements)
  * [Package requirements](#package-requirements)
  * [Logging requirements](#logging-requirements)
* [Configuration](#configuration)
* [Non-functional requirements](#non-functional-requirements)
* [Error reporting](#error-reporting)

## Overview

The [Clean-Energy-Process (CEP) Laboratory](https://www.imperial.ac.uk/clean-energy-processes/) is a laboratory, a.k.a., a research group, based at [Imperial College London](https://www.imperial.ac.uk/) with research interests varying from experimental to modelling-based approaches to problem solving. This library aims to provide both a repository of Python-based models developed by the group as well as an open-access entry point for researchers, developers and product engineers to access some of the models developed within the group in an integrated holistic way. Further, the library strives to integrate some of the disperate models from within the group into a single easy-access model.

This functional specification (FS) document aims to outline the functionality which is expected of the library and of the models contained within it.

### Reviewers

This FS document will be reviewed by [@PaulSapin](https://github.com/paulSapin).

### Modification history

Date | Comments
--- | ---
21/03/2023 | Document created

## Risks and assumptions

## Use cases

The `Technology-Library`, packaged as `ceplibrary`, aims to provide for several use cases:

1. An open-access, GitHub-hosted repository of Python-based models developed within the CEP Laboratory;
2. An interface for determining which components could fulfil a specific purpose within a system;
3. A series of open-access and peer-reviewed models of components which can be easily integrated with eachother and new models being developed in a simple "plug-and-play" style.

### :green_book: Open-access repository

First and foremost, this repository will provide an open-access interface for accessing the Python-based models developed within the CEP Laboratory. Both analytical and data-driven models will be provided within the repository. A user should be able to

* Find and browse the open-access Python-based models within the library;
* Be able to read sufficient documentation to understand the structure of, and methodology underpinning, the various models in order to both run the various models and to adapt the code to their needs.

### :crystal_ball: Component-determination interface

Within any system, there are multiple components which could, in theory, fulfil similar roles. E.G., to convert sunlight into heat, both a photovoltaic module connected to a heat pump or a solar-thermal panel could carry out this task. As more components are introduced, the number of possible component pathways increases. A user should be able to

* Request a list of components/component combinations which are capable of transforming some series of input vectors into some form of output vectors. These "energy" vectors will consist of:
  * Heat, which has some associated power flow and a temperature,
  * Energy, which has some associated power flow,
  * Irradiance, which has some assocaited energy flux,
  * Storage, which is simply a timestampt of how long, if applicable, one of these vectors should be stored within the component/system.
  
  The software will then determine which possible components can fulfil the role, limited by
  * Some target variable, e.g., the overall efficiency of the system,
  * Some maximum number of components for which a default value will be hard-baked.


### :electric_plug: Plug-and-play modules

Once a user has determined the components they want in their system, either via the method described in the [Component-determination interface](#crystal_ball-component-determination-interface) section or through some analytical means independent of the `ceplibrary`, they should be able to

* Implement any of the models contained within the library through a similar method exposed on each, i.e., a solar panel should be callable using the same methods as a heat pump such that its performance can be evaluated.

## Requirements

The code should be

* Written in [![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/);
* Open-access;
* Use a limited number of external packages where possible to widen the usability of the package;
* Conform to coding standards using
  * :art: `black`, the Python formatter;
  * :green_heart: `mypy`, the Python type-checker;
  * :shirt: `pylint`, the Python linter.

### Interface requirements

The library should expose a Python-based interface which is capable of
* Determining, given the set of inputs and outputs that a user requests, the set of available components or combinations of available components which are capable of carrying out the process requested, with some ranking based on the target criterion that a user specifies;
* Listing all components contained within the library along with the input and output energy vectors that they are capable of interfacing with.

### Package requirements

The Python package should
* Expose all technology-agnostic models for implementation;
* Expose all data-driven and analytical models contained for implementation externally;
* Provide an example script showing how these components can be coded to interface together.

### Logging requirements
Sufficient logging should take place throughout to report on
* :white_circle: `DEBUG` calls, which should provide useful information, if requested, on the internal workings of the code;
* :large_blue_circle: `INFO` calls, which should provide useful information on the flow of the code from which the flow of the code, and which sections ran, can be determined;
* :red_circle: `WARNING` calls which inform the user that the software has:
  * utilised a deprecated piece of code,
  * carried out an operation which is not recommended,
  * made an assumption based on incomplete input information;
* :black_circle: `ERROR` calls which inform the user that an error has occurred and that the flow of the code has stopped. They should contain as much information as is deemed necessary to diagnose and rectify the issue.

## Configuration

The configuration information required will vary depending on the type of model being considered:

* **Technology-agnostic models** will be instantable without any configuration provided, utilising default thermodynamically-driven values;
* **Data-driven models** will utilise data files contained within the library or from an external source in order to influence the models, again, little user configuration will be required;
* **Analytical models** will utilise inputs provided by the user when instantiating instances of the various classes which are implementable.

## Non-functional requirements

The package will contain
* Module-unit tests (MUTs) which carry out automated tests of the functionality of the various modules contained within the code;
* Component-unit tests (CUTs) which carry out component-level tests of the functionality of the various components contained within the code;
* Integration tests (ITs) which will test
  * The integrated models developed which are included in the library;
  * The component-suggesting code.

## Error reporting

If errors occur in the code, the software will, where possible, continue until the error is critical and will result in the cessation of the code. At this point, an error will be raised containing as much information as is possible and is necessary for the user to determine where the error occurred, which input file/line of code was responsible for the error, and how best to rectify the issue.
