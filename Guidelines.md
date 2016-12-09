# Hydra Plugin Development Guidelines

This document contains guidelines and expectations for developing Hydra plugins. It is one of the stated deliverables of the [Hydra Plugins Working Group](https://wiki.duraspace.org/display/hydra/
Hydra+Plugins+Working+Group).

These guidelines are supplementary, and subordinate, to existing guidelines that are documented elsewhere, including:
  * [**General contributing guidelines**](https://github.com/projecthydra/hydra/blob/master/CONTRIBUTING.md)
  * [**Project promotion process**](http://projecthydra-labs.github.io/promotion.html)


## Architecture

1. Plugins **must** be Ruby gems.

1. Plugins **should** be Railties or Rails engines unless there is good reason to do otherwise.

1. Plugins **must** have a unique namespace under which classes and modules are defined.

1. Plugins **should** use [semantic versioning](http://semver.org).

1. Plugins **should** follow established Hydra coding conventions and design patterns wherever applicable.

1. Instance methods that are not part of the plugin's interface **should not** be public.

## Installation

1. Plugins **should** use Rails generators for making changes to their host Hydra application.

## Handling installation failures

1. Plugins **should** print meaningful error messages in case of a failure during installation.

1. Plugins **should** abort installation if required dependencies are missing.

1. Plugins **should** undo any changes made to the host application during a failed installation.

1. Plugins **should** provide information on any changes made to the host application that could not be undone after a failed installation.

## Installation documentation

1. Installation instructions for plugins **should** include a list of all available installation options with descriptions of how they affect the installation.

1. Plugin installation instructions **should** include screen shots of any expected changes to the UI.

## Error handling

1. Plugins **should** use custom error classes when raising errors. This allows implementers to easily identify a raised exception as coming from the plugin, as well as providing a convenient way to rescue from specific exceptions as needed.

1. Custom error messages **should** include tips on how to fix the error when such information is known.

### Interface documentation

1. All public instance methods under the plugin's namespace are considered to be part of the plugin's interface, and should be documented.

1. All class methods and module methods intended to be invoked by the host application are considered to be part of the plugin's interface, and **should** be documented.

1. All configuration files and settings that affect the plugin's behavior are considered to be part of the plugin's interface, and should be documented.

### Tutorials

1. Plugin documentation **should** include a tutorial on how to use each feature the plugin offers.