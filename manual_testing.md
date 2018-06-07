---
title: Manual Testing
permalink: /manual-testing/
---

When testing a theme manually, here are some things to check.

## Checklist

### Accessibility

- Does all text meet [WCAG standards](http://webaim.org/resources/contrastchecker/)?
- Does the site function properly when accessed in a browser for the blind or the visually impaired?
- Does the site [validate](https://validator.w3.org/)?

### Aesthetics

- ...

### Flexibility

- Does theme handle small, large, portrait, and landscape logos without distorting, inappropriately upscaling or downscaling, or serving inappropriately large versions of the logo?
- Does the site handle the extremes of content well? Lots of content? No content? Lost of icons? No icons? Etc.
- Does the theme still function properly given any combination or extreme of user-supplied configuration values?
- What happens if the user sets all theme colors to white? Black?

### Functionality

- Are emails linked using `mailto:` and phone numbers linked using `tel:`?

### Graceful Degradation

- Does the site function properly in all browsers?
- Is the site still usable without JavaScript? Without CSS?

### Performance

- Are small versions of large images served on mobile?
- Does content jump around when [connection speed is throttled](https://css-tricks.com/throttling-the-network/)?

### Responsiveness

- Are small versions of large images served on mobile?
- Does content jump around when [connection speed is throttled](https://css-tricks.com/throttling-the-network/)?
- Does every page and component function properly [at any screen size](https://developers.google.com/web/tools/chrome-devtools/device-mode/?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3)?
- Do retina devices receive images which are twice as wide as the image's rendered display width?

## Testing in Multiple Browsers Using Browsersync

[Browsersync](https://www.browsersync.io/) allows you to sync scrolling, clicking, live reloading, etc, between multiple windows and browsers. It makes testing multiple screen widths and browsers much simpler.

To use Browsersync, after installing, navigate to `SUFramework/cache/` in Terminal and run the following command:

```
browser-sync start --proxy "localhost:8080" --files "css/**/*.css" "twig/**/*.php"
```

Then open `http://localhost:3000` in as many browsers, windows, or devices on the same network as you wish to test in.

## See Also

- [External Resources](external_resources.md)
