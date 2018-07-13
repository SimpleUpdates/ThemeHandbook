---
title: Manual Testing
permalink: /manual-testing/
---

When testing a theme manually, here are some things to check.

## Checklist

### Accessibility

- [ ] All text meets [WCAG standards](http://webaim.org/resources/contrastchecker/)
- [ ] Site functions properly when accessed in a browser for the blind and the visually impaired
- [ ] Site uses the sitename for logo alt text
- [ ] Site [validates](https://validator.w3.org/)

### Aesthetics

- [ ] Theme uses attractive placeholder images for carousels, image buttons, etc

### Flexibility

- [ ] Theme handles small, large, portrait, and landscape logos without distorting, inappropriately upscaling or downscaling, or serving inappropriately large versions of the logo
- [ ] Site handles the extremes of content well—lots of content, no content, lots of icons, no icons, etc.
- [ ] Theme still functions properly given any combination or extreme of user-supplied configuration values
- [ ] Theme remains usable and all text is readable when the user sets all theme colors to white or black

### Functionality

- [ ] Email addresses are linked using `mailto:` and phone numbers using `tel:`

### Graceful Degradation

- [ ] Site functions properly in all supported browsers
- [ ] Site is usable without JavaScript and/or CSS

### Performance

- [ ] Small versions of large images are served on mobile
- [ ] Content doesn't jump around while images load when [connection speed is throttled](https://css-tricks.com/throttling-the-network/)

### Responsiveness

- [ ] Small versions of large images are served on mobile
- [ ] Content doesn't jump around while images load when [connection speed is throttled](https://css-tricks.com/throttling-the-network/)
- [ ] Every page and component functions properly [at any screen size](https://developers.google.com/web/tools/chrome-devtools/device-mode/?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3)
- [ ] Retina devices receive images which are twice as large as the image's rendered display dimensions

## Testing in Multiple Browsers Using Browsersync

[Browsersync](https://www.browsersync.io/) allows you to sync scrolling, clicking, live reloading, etc, between multiple windows and browsers. It makes testing multiple screen widths and browsers much simpler.

To use Browsersync, after installing, navigate to `SUFramework/cache/` in Terminal and run the following command:

```
browser-sync start --proxy "localhost:8080" --files "css/**/*.css" "twig/**/*.php"
```

Then open `http://localhost:3000` in as many browsers, windows, or devices on the same network as you wish to test in.

## See Also

- [External Resources](external_resources.md)
