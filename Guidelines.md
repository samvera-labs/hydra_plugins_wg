# Hyrax Plugin Development Guidelines

This document contains guidelines and expectations for developing Hyrax plugins. It is one of the stated deliverables of the [Hydra Plugins Working Group](https://wiki.duraspace.org/display/hydra/Hydra+Plugins+Working+Group)

_(NOTE: the working group was started before Project Hydra changed it's name to Samvera)._

These guidelines are supplementary and subordinate to existing guidelines that are documented elsewhere, including:
  * [**General contributing guidelines**](https://github.com/projecthydra/hydra/blob/master/CONTRIBUTING.md)
  * [**Project promotion process**](http://projecthydra-labs.github.io/promotion.html)

## What is a "Hyrax plugin"?

A Hyrax plugin is a Ruby gem that interacts with the Hyrax framework in the context of a Rails application.

## Architecture

1. A plugin **must** be a Ruby gem.

   **Justification:** Gems are the standard way to share Ruby code.

1. A plugin **should** be a [Railtie](http://api.rubyonrails.org/classes/Rails/Railtie.html) if it needs to interact with the Rails framework.

   **Justification:** Railties provide a standard way of interacting with the Rails framework.

1. A plugin **may** be a [Rails Engine](http://api.rubyonrails.org/classes/Rails/Engine.html) if it provides models, views, controllers, routing, or other other Rails application entities.

   **Justification:** Rails Engines provide a structure that is consistent with Rails apps.

1. Plugins **must** have a unique namespace under which classes and modules are defined.

1. Plugins **should** use [semantic versioning](http://semver.org).

1. Plugins **should** follow established Hyrax coding conventions and design patterns wherever applicable.

1. Instance methods that are not part of the plugin's interface **should not** be public.

1. Plugins that are mounted Rails Engines **must** specify the routing proxy when using URL helpers created by Rails routes.

   **Justification:** In the context of Rails Engines, the default routing proxy depends on your context. Typically, it will be either the routing proxy for the main application, or the routing proxy for the engine. By default, Rails creates helper methods for accessing the routing proxies within your views. The helper method for the main applications routing proxy is `main_app`, while the helper method for the routing proxy of your engine is the engine's name, downcased and underscored, e.g. if your engine is named `MyCoolPlugin`, then the routing proxy can be accessed via the helper method `my_cool_plugin`.

   This guideline is meant to reinforce the [existing guidelines for using routing proxies in Rails Engines](http://edgeguides.rubyonrails.org/engines.html#routes). Adhering to this guideline reduces potential conflict when using url helpers across different Hyrax plugins.

1. Plugins **should not** overwrite classes or modules from other gems.

   **Justification:** Overwriting classes, modules, or methods from other gems violates the [open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle).

## Installation

1. Plugins **should** use Rails generators for making changes to their host Hyrax application.

   **Justification:**

     * Generators are a proven and robust standard for allowing gems to modify host applications and provide feedback on changes that have occurred.

     * Generators provide a conventional location for implementers to explore and debug the code that is makes changes to a host application.

1. Plugins **should** only generate or insert code into the host application when necessary.

   **Justification:** Generated code in a client application represents a significant, and often invisible, public interface for the plugin. Such code is brittle and can easily be broken by point release of a plugin, in violation of the spirit of Semantic Versioning. This can make plugins a significant source of maintenance burden for implementers, who must constantly audit and fix code they did not write and may not be familiar with, when upgrading dependencies. Ideally, a plugin should prefer to keep code, configuration, and initializers isolated within the plugin's gem, allowing implementers to copy over files themselves only if they find it necessary for customization purposes.

## Handling installation failures

1. Plugins **should** print meaningful error messages in case of a failure during installation.

   **Justification:** It is always a good idea to provide implementers with as much meaningful information as possible, so that they can either resolve the issue themselves, or provide a meaningful error message to the community or in a bug report when seeking help. Plugins should prefer to fail at installation time rather than later on if at all possible.
   
1. Plugins **should** abort installation if required dependencies are missing.

1. Plugins **should** undo any changes made to the host application during a failed installation.

   **Justification:** It becomes much easier for the community to adopt plugins if they can rely on plugins to be good citizens. Cleaning up after a failed installation means that implementers can have faith that their codebase won't be left in a broken state, especially in light of other guidelines which encourage plugins to "fail fast" at installation time.

1. Plugins **should** provide a summary of changes made to the host application in a post-install message.

  **Justification:** Plugin implementers need to know what has changed in the host application so they can undo those changes if necessary.

1. Plugins **should not** overwrite classes or modules in the host application.

   **Justification:** Overwriting classes or modules may change behavior expected by the host application, other plugins, or third party gems.

## Installation Documentation

The term **installation** in this context means all steps required to enable the plugin within the host application. This may include, but is not limited to:

  * Rails generators
  * ad-hoc scripts
  * manual setup instructions

1. Installation documentation for plugins **should** include a list of all available installation options with a description of their effects.

  **Justification:** It should not be left up to the implementer to figure out the various ways a plugin may be installed.

1. Plugin installation instructions **should** include screen shots of any expected changes to the UI.

   **Justification:** Screen shots serve as a visual confirmation to the implementer that the plugin is working as advertised. Textual descriptions of UI changes are encouraged as well, but oftentimes pictures are more useful, and more concise.

## Error handling

1. Plugins **should** use custom error classes when raising errors.

   **Justification:** Standard or default exception classes do not provide plugin-specific information about the error. Custom error classes allow implementers to easily identify a raised exception as coming from the plugin, as well as providing a convenient way to rescue from specific exceptions as needed.

1. Custom error messages **should** include tips on how to fix the error when such information is known.

   **Justification:** Tips help the user recover more quickly from the error condition and increase user confidence. Adding tips directly to the plugin eases the support requirements on the larger community.

## Interface documentation

The term **interface** in this context means all the ways in which the host application may invoke, or alter, a plugin's behavior. This includes, but may not be limited to:
  * All public instance methods under the plugin's namespace.
  * All class methods and module methods that plugin is expecting to be called from the context of the host application.
  * All configuration files and settings that live in the host application and affect the plugin's behavior.

1. A plugin's interface **must** be documented.

   **Justification:** Without documentation, plugin adopters will not understand the plugin interface's expectations and usage.
  
1. Public instance methods, class methods, and module methods that are part of the plugin's interface **must** be documented with [YARD](http://yardoc.org/), using `@api` tags to indicate which methods are part of the plugin's interface.

   **Justification:** The use of the `@api` tag is a convenient way for plugin authors to define methods intended to be used by downstream adopters, and conforms to existing Hyrax contribution guidelines that specify including inline documentation in YARD.

## Tutorials

1. Plugin documentation **should** include a tutorial on how to use each feature the plugin offers.

   **Justification:** Tutorials help to inform users, and potential users, about all of the features that a plugin offers. More importantly, they provide instruction on installation and configuration that would otherwise be difficult to discover.
