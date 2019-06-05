---
title: Components
permalink: /components/
---

## Layers

Components are grouped into four distinct layers:

- **Atoms:** "The foundational building blocks that comprise all our user interfaces. These atoms include basic HTML elements 
like form labels, inputs, buttons, and others that can’t be broken down any further without ceasing to be functional."
- **Molecules:** "Relatively simple groups of UI elements functioning together as a unit. For example, a form label, search 
input, and button can join together to create a search form molecule."
- **Organisms:** "Relatively complex UI components composed of groups of molecules and/or atoms and/or other organisms. These 
organisms form distinct sections of an interface."
- **Layouts:** "Place components within a layout and demonstrate the design’s underlying content structure."

(Definitions taken from *Atomic Design*, by Brad Frost, [chapter 2](http://atomicdesign.bradfrost.com/chapter-2/).)

## Benefits

Breaking themes into components has several benefits:

- It becomes clear where to look to find all the Less or Twig source related to any given component.
- Components become more portable, as using a component architecture forces a clear separation between the source code of one
component and another, and allowing any given component to be used in many contexts.
- This clear separation between components makes modifying a single component easier, as it allows for the assumption that the
source of one component will not affect any other component outside its context.
- Breaking a theme into components keeps files short, making them easier to understand and modify.
- Because they are context agnostic, components can be displayed in a styleguide for easier and more-flexible testing.
- Components help keep things DRY ([Don't Repeat Yourself](http://programmer.97things.oreilly.com/wiki/index.php/Don%27t_Repeat_Yourself)) by reducing code duplication, thereby increasing theme maintainability.
- Source complexity is more easily controled when everything related to a given component can be viewed at the same time. After consolidating all the CSS for a given component into one Less file, unneeded complexity and duplication which was almost impossible to detect and safely remove often becomes painfully obvious and simple to address.

### Trivial Components

But what about trivial components, like headings, hard rules, and links? Is it worth it to break out the styling for these components into separate files instead of simply keeping the styles in global.less?

There are still good reasons to use separate files for these basic components:

1. **Readability.** Keeping the styling for each component in its own file keeps `global.less` from growing longer and longer over time, making it and the individual style files it imports easier to read and understand.

2. **Decoupling.** Keeping the styling for each component in its own file prevents the styling for different components from becoming intertwined. When styling for a single component is scattered througout a theme's stylesheets, reasoning about that component is difficult. When a single style rule may affect multiple components, making changes to a theme becomes dangerous. Keeping styles explicitly namespaced in separate files guards against this type of coupling.

3. **Maintainability.** When each component has its own file, it's easier to ensure that each component is properly namespaced; that is, its outer-most element has a class with the component's name (`atom-rule` or `organism-footer`). When a component is properly namespaced, you can be more confident that all the code in that file only affects that one component, and that changes you make to that component's styling won't impact other components that haven't explicitly used that component.

#### An Example

To demonstrate how styling for even simple components can become difficult to maintain over time, let's look at an example. Say I'm building a theme, and I want all the headings from `h1` to `h6` to have a red line beneath them. I write the following styling in `global.less`:

```
h1, h2, h3, h4, h5, h6 { 
    border-bottom: 1px solid red; 
}
```

As I continue developing the theme, I decide I want hard rules to mirror the styling for headings, so I modify `global.less` like so:

```
h1, h2, h3, h4, h5, h6, hr { 
    border-bottom: 1px solid red; 
}
```

At this point I've already begun to couple two components, `atom-heading` and `atom-rule` we might call them, by using a CSS selector that targets both components. If later I want to change the color of the underline on headings but not on hard rules, I'll first have to decouple the styling by breaking the selector block into two. This is a small example of the maintainability costs of this kind of coupling. 

But it probably won't end with the two components becoming coupled. As I continue developing my theme, since I've already started down the path of storing styling in `global.less`, more styling will probably accumulate there. And, at some point, I'm likely to decide that I want to make first-level headings bold, so I add this to the end of the stylesheet:

`h1 { font-weight: bold; }`

Now, not only is the styling for `atom-heading` and `atom-rule` coupled, but the styling for `atom-heading` is not in a single location. This fragmenting of the styling for a single component tends to lead to the accumulation of conflicting and complex styling, as developers making modifications to the theme in the future are not likely to be aware of all the different locations where styling exists to modify this single component.

## File Structure

Each component has a name in the form of `.layer-componentIdentifier`. (For more information on naming components, see 
[naming_things.md](https://github.com/SimpleUpdates/ThemeHandbook/blob/master/naming_things.md).) This name is used for a 
component's Twig partial, Less file, and base CSS classname.

All component Twig partials are placed in the theme's `partial/` folder. All component Less files are placed in the theme's
`style/` folder, and included at the end of `global.less`, grouped by layer and sorted alphabetically, within `.su_bootstrap_safe` if the theme uses Bootstrap.

```css
// Variable declarations ...

.su_bootstrap_safe
  // Global CSS [minimize and eliminate if at all possible] ...

  @import "atom-button";
  @import "atom-search";

  @import "molecule-branding";
  @import "molecule-breadcrumbs";
  @import "molecule-carousel";
  @import "molecule-nav";
  @import "molecule-icons";

  @import "organism-footer";
  @import "organism-navBar";
}
```

Note that, even though each component Less file filename should end with `.less`, it's not necessary to include the 
extension when importing the file in `global.less`.

## Smells

> "A code smell is a surface indication that usually corresponds to a deeper problem in the system" ([Martin Fowler](https://www.martinfowler.com/bliki/CodeSmell.html)).

If you notice any of the following in a theme, it's likely that the theme hasn't been properly broken into components.

- **A component's Less file contains CSS that's only relevant in a specific context.** A component's Less file should only contain CSS that would be relevant to that component no matter where in a theme it's used. Whenever you see CSS in a component's Less file that relates to its use at a specific location in the theme or another component, that CSS should be moved into a location specific to that context (such as a parent component's less file or `global.less`).
- **A component's Less file or Twig file is not wrapped with its primary class.** In order to keep components as portable as possible, it's useful to apply the component's primary class name (in the form of `.layer-componentIdentifier`) to its outermost element, and to wrap everything in its Less file, including variable declarations, with the same class. This practice increases the confidence we can have that a given component's CSS will only ever affect that specific component.
- **A component's CSS references variables not declared in the same file.** Generally, by redeclaring any needed global variables within the component's primary class scope, and only using those redeclared variables in the component's CSS, the component is made more portable, easier to customize, and more readable. (For more information on naming these redelcared variables, see [naming_things.md](https://github.com/SimpleUpdates/ThemeHandbook/blob/master/naming_things.md).) One exception to this recommendation is global breakpoint variables (e.g. `@screenSmMax`), which should be referenced without being redeclared. 
- **A component's Twig source doesn't make sense alone.** If you see an orphaned `</div>` at the beginning of a component's Twig file, or an unclosed element at its end, that's a strong indication that the component hasn't been properly decoupled
from other components or templates, and needs to be refactored so it can stand alone.
- **Component-specific media queries appear in `global.less`.** Any media queries that relate to a given component regardless of its context should be kept in that component's Less file.
- **Print-specific CSS rules appear inside a component's Less file.** The one exception to the previous smell is print media queries, which should only appear in `global.less`; or, preferrably, in a separate `print.less` file.
- **Less source makes reference to specific pages, often through the use of classes like `.layout-home`.** Whenever possible, components should be made entirely portable. Coupling a component's style to its context is usually an antipattern. Solutions to this smell include breaking the component into multiple variations, moving the context-specific styling out of the component and into the context it's meant to target, and adding import parameters to the Twig file.
- **A component's Less file contains multiple top-level classes.** Allowing a component's CSS to escape its component class leads to the possibility that a change to a component's styling may affect something outside that component.
