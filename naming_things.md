---
title: Naming Things
permalink: /naming-things/
---

> "There are only two hard things in Computer Science: cache invalidation and naming things" (Phil Karlton, [via Martin Fowler](https://www.martinfowler.com/bliki/TwoHardThings.html)).

Good names are important, because code is only easy to change when it's easy to read and understand.

## General

- Prioritize understandable over short names.
- Avoid abbreviations and otherwise-unpronouncable names.
- If something is a collection of other things, name it as the plural of what it contains.
- If something is a single thing, give it a singular noun name.
- If something is a function, give it a verb name.
- Aim for names that allow your code to read like well-formed prose.

## Naming Less Variables

Less variable names should be camel cased.

| Yes             | No
|-----------------|-----------------
| `@variableName` | `@VariableName`
|                 | `@variable_name`
|                 | `@variable-name`

Less variable names depend on context.

Less variables that hold user-provided values should mirror the name of the config property.

| Yes                                 | No
|-------------------------------------|----------------------------------
| `@accentColor: @config-accentColor` | `@mainColor: @config-accentColor`

All other Less variables defined in `global.less` should reflect their content, not their intended use.

| Yes             | No
|-----------------|------------------------
| `@greyLight`    | `@borderColor`
| `@accentMedium` | `@linkColor`
| `@tan`          | `@carouselButtonColor`

Less variables defined in components, on the other hand, should be named in reference to their use, and not their content.

| Yes                          | No
|------------------------------|---------------------------
| `@linkColor: @accentDark`    | `@buttonGrey: @greyMedium`

## Naming CSS Classes

Most classes should be named according to the component they are a part of. A component's basename should take the form of `.layer-componentName` in order to mirror the component's file name. Its internal classes should be of the form `.layer-componentName__subclass`. Any class which does not pertain to a specific component should be hyphenated, though the need for its existense at all should be questioned.

| Yes                               | No
|-----------------------------------|-----------------
| `.class-name`                     | `@className`
| `.organism-navBar`                | `@class_name`
| `.molecule-contentHeader__byline` | `@ClassName`

## Naming Components

Components are grouped into four distinct layers:

- **Atoms:** "The foundational building blocks that comprise all our user interfaces. These atoms include basic HTML elements 
like form labels, inputs, buttons, and others that can’t be broken down any further without ceasing to be functional."
- **Molecules:** "Relatively simple groups of UI elements functioning together as a unit. For example, a form label, search 
input, and button can join together to create a search form molecule."
- **Organisms:** "Relatively complex UI components composed of groups of molecules and/or atoms and/or other organisms. These 
organisms form distinct sections of an interface."
- **Layouts:** "Place components within a layout and demonstrate the design’s underlying content structure."

(Definitions taken from *Atomic Design*, by Brad Frost, [chapter 2](http://atomicdesign.bradfrost.com/chapter-2/).)

A component's name takes the form of its layer, followed by a hyphin, followed by its specific name in camel case:

| Yes                 | No
|---------------------|-----------------
| `atom-button`       | `atom-Button`
| `molecule-branding` | `branding`
| `organism-navBar`   | `org-nav-bar`
| `layout-default`    | `default`

A component's name is used in three primary places: its Twig partial filename, its Less file filename, and its primary CSS
class. All three should be identical, with the possible exception of legacy layout Twig files.

| Yes                                                                | No
|--------------------------------------------------------------------|------------------------------------------------------
| `organism-navBar.html`, `organism-navBar.less`, `.organism-navBar` | `organism-NavBar.html`, `navBar.less`, `.org-nav-bar`

