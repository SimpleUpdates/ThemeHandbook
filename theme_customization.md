---
title: Theme Customization
permalink: /theme-customization/
---

This document is for the purpose of outlining best practices when modifying a theme's source files for a specific website.

## Pitfalls

- Editing a theme's source files removes the theme from automatic updates.
- Merging in new updates once a theme has been customized can be difficult, as conflicts between customizations and theme 
  updates are likely to occur.

## Best Practices

- As much as possible, keep all customizations in the site's Custom CSS field. Even customizations which, on their face, require editing Twig files can often be done entirely, or almost entirely, with only CSS overrides in `custom.less`.
- Avoid editing other `.less` files even more than you avoid editing `.twig` files. `.less` files are
  highly likely to create merge conflicts.
- Keep a list of customizations made as an aid to future merges.
- Try to merge in updates on a semi-regular basis.

## Editing Source Files

0. Log into your website
0. Click "Themes" (the paint brush) in the black admin toolbar
0. Underneath the name, description, and customize button of your active theme, click the text link "Advanced HTML Editor"

## Merging Updates

Merging in updates to a customized theme is generally not something that can be done without the help of SimpleUpdates staff,
as it requires command line access. SimpleUpdates staff use the following tools when merging in updates:

- [OpenDiff](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/opendiff.1.html) 
  ([guide to using with Git](https://gist.github.com/bkeating/329690))
- [Git](https://git-scm.com/)
