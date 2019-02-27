---
title: Components
permalink: /components/
---

## Layers

Components are grouped into three distinct layers:

- **Atoms:** "The foundational building blocks that comprise all our user interfaces. These atoms include basic HTML elements 
like form labels, inputs, buttons, and others that can’t be broken down any further without ceasing to be functional."
- **Molecules:** "Relatively simple groups of UI elements functioning together as a unit. For example, a form label, search 
input, and button can join together to create a search form molecule."
- **Organisms:** "Relatively complex UI components composed of groups of molecules and/or atoms and/or other organisms. These 
organisms form distinct sections of an interface."

(Definitions taken from *Atomic Design*, by Brad Frost, [chapter 2](http://atomicdesign.bradfrost.com/chapter-2/).)

## Benefits

Breaking themes into components has several benefits:

- It becomes clear where to look to find all the Less or Twig source related to any given component.
- Components become more portable, as using a component architecture forces a clear separation between the source code of one
component and another, and allowing any given component to be used in many contexts.
- This clear separation between components makes modifying a single component easier, as it allows for the assumption that the
source of one component will not affect any other component outside its context.
- Breaking a theme into components keeps its layout files and `global.less` file short, making them easier to understand and
modify.
- Because they are context agnostic, components can be displayed in a styleguide for easier and more-flexible testing.
- Components help with keeping things DRY ([Don't Repeat Yourself](http://programmer.97things.oreilly.com/wiki/index.php/Don%27t_Repeat_Yourself)) by eliminating code duplication, thereby increasing theme maintainability.
- Source complexity is much more easily controled when everything related to a given component is easily viewed at the same time. After consolidating all the CSS for a given component into one Less file, unneeded complexity and duplication which was almost impossible to detect and safely remove often becomes painfully obvious and trivial to remove.

But what about trivial components, like headings, hard rules, and links? Is it worth it to break out the styling for these components into separate files instead of simply keeping the styles in global.less?

There are still a couple of good reasons to continue break out these trivial components:

1. **Readability.** Keeping each component in its own file means generally shorter files (global.less used to get quite long). Also, when each component has its own file, reasoning about that component is easier, not only because the file is shorter, but also because you can see all the code for that component in one place. Before we did this, code that affected a single thing (component, loosely) would end up getting scattered throughout global.less, and it was often hard to have any kind of confidence that you were looking at everything pertaining to it.

2. **Maintainability.** When each component has its own file, it makes it much easier to ensure that each component is properly namespaced (meaning its outer-most element has a class with the component's name). When a component is properly namespaced, it means you can be more confident that all the code in that file only affects that one component, and that changes you make to that component's styling won't leak out and touch other components that haven't explicitly used that component.

So the reason even these trivial components have their own style files is to make it very clear what they are (trivial components) and very easy to verify that we aren't doing anything that would impair readability or maintainability.

For instance, say you had both `hr` (often the rule component) and `h1`-`h6` (often the heading component) in global.less. You could easily see styling be written like this:

`hr, h1, h2, h3, h4, h5, h6 { border-bottom: 1px solid red; }`

Then later you could see more styling being added to the headings in another place in global.less:

`h1 { font-weight: bold; }`

This would already start reducing the readability of the code by making it unclear where to go to see all the styling for headings. Keeping even styling for trivial components in separate files guards against such regressions.

## File Structure

Each component has a name in the form of `.layer-componentIdentifier`. (For more information on naming components, see 
[naming_things.md](https://github.com/SimpleUpdates/ThemeHandbook/blob/master/naming_things.md).) This name is used for a 
component's Twig partial, Less file, and base CSS classname.

All component Twig partials are placed in the theme's `partial/` folder. All component Less files are placed in the theme's
`style/` folder, and included at the end of `global.less`, grouped by layer and sorted alphabetically, within `.su_bootstrap_safe`, if the theme uses it.

```css
// Variable declarations ...

.su_bootstrap_safe
  // Global CSS ...

  @import "styleguide";

  @import "atom-button";
  @import "atom-search";

  @import "molecule-branding";
  @import "molecule-breadcrumbs";
  @import "molecule-carousel";
  @import "molecule-nav";
  @import "molecule-icons";

  @import "organism-footer";
  @import "organism-navBar";

  @import "custom";
}
```

Note that, even though each component Less file filename should end with `.less`, it's not necessary to include the 
extension when importing the file in `global.less`.

## Smells

> "A code smell is a surface indication that usually corresponds to a deeper problem in the system" ([Martin Fowler](https://www.martinfowler.com/bliki/CodeSmell.html)).

If you notice any of the following in a theme, it's likely that the theme hasn't been broken into components properly.

- **A component's Less file contains CSS that's only relevant in a specific context.** A component's Less file should only contain CSS that would be relevant to the component no matter where in a theme it's used. Whenever you see CSS in a component's Less file that relates to its use at a specific location in the theme or another component, that CSS should be moved into a location specific to that context (such as a parent component's less file or `global.less`).
- **A component's Less file or Twig file is not wrapped with its primary class.** In order to keep components as portable as possible, it's useful to apply the component's primary class name (in the form of `.layer-componentIdentifier`) to its outermost element, and to wrap everything in its Less file, including variable declarations, with the same class. This practice increases the confidence we can have that a given component's CSS will only ever affect that specific component.
- **A component's CSS references variables not declared in the same file.** Generally, by redeclaring any needed global variables within the component's primary class scope, and only using those redeclared variables in the component's CSS, the component is made more portable and easier to customize. (For more information on naming these redelcared variables, see [naming_things.md](https://github.com/SimpleUpdates/ThemeHandbook/blob/master/naming_things.md).) One exception to this rule is global breakpoint variables (e.g. `@screenSmMax`), which should be referenced without being redeclared. 
- **A component's Twig source doesn't make sense alone.** If you see an orphaned `</div>` at the beginning of a component's Twig file, or an unclosed element at its end, that's a strong indication that the component hasn't been properly decoupled
from other components or templates, and needs to be refactored so it can stand alone.
- **Component-specific media queries appear in `global.less`.** Any media queries that relate to a given component regardless of its context should be kept in that component's Less file.
- **Print-specific CSS rules appear inside a component's Less file.** The one exception to the previous smell is print media queries, which should only appear in `global.less`; or, preferrably, a separate `print.less` file.
- **Less source makes reference to specific pages, often through the use of classes like `.home`.** Whenever possible, components should be made entirely portable. Coupling a component's style to its context is usually an antipattern, and indicates either a need to break the component into multiple variations, or the need to move the context-specific styling out of the component and into the context it's meant to target.
- **A component's Less file contains multiple top-level classes.** Allowing a component's CSS to escape its top-level containing class leads to the possibility that a change to a component's styling may affect something outside that component.
