---
title: Less API
permalink: /less-api/
---

Item                                           | Description
-----------------------------------------------|-------------------------
`@config-themeCustomizerVariableName`          | Access theme customizer variable values. Ex.: `@config-backgroundColor`
`@su-assetpath`                                | Theme asset folder path. Ex.: `background:url('@{su-assetpath}/image.png');`
`.theme-image-fit(@which; @width; @height;)`   | Path to image resized to fit within provided dimensions.
`.theme-image-fill(@which; @width; @height;)`  | Path to image cropped to fill provided dimensions. Won't crop small images.
`.theme-image-ratio(@which; @width; @height;)` | Path to image cropped to match provided ratio. Will crop small images.

When referring to images via `@config-*`, the variable must be used in the context of one of the `.theme-image-*` mixins. Example:

```less
.background {
	  .theme-image-ratio( @config-bannerBg; 2000px; 460px; );
	  background-size: cover;
	  background-position: 50%;
}
```
