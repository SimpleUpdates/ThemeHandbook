---
title: Twig API
permalink: /twig-api/
---

## General

Item                                  | Description
--------------------------------------|--------------
`su.calendar.events`                  | `event.id`, `event.title`, `event.description`, `event.start`, `event.end`, `event.location`, `event.isAllDay`
`su.content`                          |
`su.footer`                           |
`su.misc.privatelabel`                | Retrieve site's private label
`markdown_content\|markdown` | The `markdown` filter allows you to convert MarkDown to HTML. It's important to output the resulting output using the `raw` filter to output the resulting HTML; otherwise it will be escaped.

## Images

Item                   | Description
-----------------------|--------------
`.fit(x,y)`            | Resizes image down to fit the given dimensions. Does not upscale.
`.fit(x)`              | Constrain only width
`.fit(null,y)`         | Constrain only height
`.fill(x,y)`           | Crops image to fit the given dimensions. Does not crop if the image is already smaller than the given dimensions.
`.ratio(x,y)`          | Always crops image to match the ratio of the given dimensions, regardless of image's resolution.
`.focalPoint`          | `.focalPoint.x`, `.focalPoint.y`; returns percent integer without the `%` sign.

## su.calender

Item                                        | Description
--------------------------------------------|-----------------------------------
`su.calendar.sunsetTime`                    | Get sunset time for today
`su.calendar.sunsetTime( "+1 day" )`        | Get sunset time for tomorrow
`su.calendar.sunsetTime( "next friday" )`   | Get sunset time for next Friday
`su.calendar.sunsetTime( "next saturday" )` | Get sunset time for next Saturday
`su.calendar.sunsetTime|date( "H:i:s" )`    | Format returned time

## su.collection

Item                                                                          | Description
------------------------------------------------------------------------------|-----------------------------------------
`su.collection( (string) collectionId )`                                      | Fetch collection
`su.collection( "id" ).find()`                                                | Fetch items out of collection
`su.collection( "id" ).state("MT").find()`                                    | Filter where `state` field is "MT"
`su.collection( "id" ).state("M%").find()`                                    | Filter where `state` field starts with "M"
`su.collection( "id" ).state("M%").which("Two").find()`                       | Filters are chainable
`su.collection( "id" ).state("M%").or().which("Two").find()`                  | Filter for this *or* that
`su.collection( "id" ).number(2, ">").find()`                                 | Comparison operators: `=`, `!=`, `<>`, `>`, `<`, `>=`, `<=`
`su.collection( "id" ).sort( (string) fieldId ).find()`                       | Sort by specified field
`su.collection( "id" ).sort( "state", "desc" ).find()`                        | Sort and specify sort direction
`su.collection( "id" ).sort( "name.last", "desc" ).find()`                    | Sort by sub field
`su.collection( "id" ).randomize.find()`                                      | Sort randomly
`su.collection( "id" ).state("MT").first()`                                   | Fetch only first result
`su.collection( "id" ).state("MT").count()`                                   | Fetch result count
`su.collection( "id" ).state("MT").limit(2).find()`                           | Fetch only two results
`su.collection( "id" ).state("MT").offset(2).find()`                          | Fetch third result and onward
`su.collection( "id" ).activationDate( "now"\|date( 'Y-m-d' ), "<=" ).find()` | Compare with a date
`su.collection( "id" ).deactivationDate( "", "=" ).find()`                    | Assert null date
`su.page.urlForCollectionItem( item )`                                        | Retrieve the URL for an item's detail page
`su.page.collectionItem()`                                                    | Retrieve current item on collection item detail page

Example usage:

{% raw %}
```twig
{% set button_items = su.collection( "gumwoodButtons" )
		.activationDate( "now"|date( 'Y-m-d' ), "<=" )
		.deactivationDate( "now"|date( 'Y-m-d' ), ">" ).or().deactivationDate( "", "=" )
		.find() %}
```
{% endraw %}

## su.color

### Color Definition Functions

Item                      | Description
--------------------------|--------------------------------------------------
`su.color.hsl( h, s, l )` | Create an HSL color object

### Color Channel Functions

Item                           | Description
-------------------------------|--------------------------------------------------
`su.color.hue( color )`        | Get a color's hue
`su.color.saturation( color )` | Get a color's saturation

### Color Operation Functions

Item                                                       | Description
-----------------------------------------------------------|--------------------------------------------------
`su.color.saturate( color, amount )`                       | Saturate a color
`su.color.desaturate( color, amount )`                     | Desaturate a color
`su.color.lighten( color, amount )`                        | Lighten a color
`su.color.darken( color, amount )`                         | Darken a color
`su.color.fadein( color, amount )`                         | Decrease opacity
`su.color.fadeout( color, amount )`                        | Increase opacity
`su.color.fade( color, amount )`                           | Set opacity
`su.color.spin( color, angle )`                            | Spin hue
`su.color.mix( color1, color2, weight )`                   | Mix two colors
`su.color.tint( color, weight )`                           | Mix a color with white
`su.color.shade( color, weight )`                          | Mix a color with black
`su.color.grayscale( color )`                              | Completely desaturate a color
`su.color.contrast( color, [dark], [light], [threshold] )` | Select a contrasting color (dark & light default to black & white)

## su.editable

Item                                        | Description
--------------------------------------------|--------------------------------------------------
`su.editable.region( (string) identifier )` | Allows inline editing of a custom content region
`su.editable.title`                         | Allows inline editing of page title

Editable region identifer strings are most comonly named `custom1`, `custom2`, `custom3`, etc. `custom1` almost always coincides with a layout's sidebar. Whenever possible, stick to this naming convention so that a user's page content remains visible when they switch to a layout with a sidebar in another theme.

## su.media

Item                                              | Description
--------------------------------------------------|-------------------------------
`su.media`                                        | Array of media items
`su.media.tagGroup("Group Name").tag("Tag Name")` | Array of media items associated with specified tag
`mediaItem.assets`                                | Array of assets
`asset.url`                                       | Asset's URL

## su.page

Item                                   | Description
---------------------------------------|-------------------------------         
`su.page("/url")`                      | Get page by path
`su.page.authors`                      |
`su.page.breadcrumbs`                  |
`su.page.children`                     | Get a page's children
`su.page.children(id)`                 | Get a specific page's children. Inspect the /admin/page/list table to get the id.
`su.page.content`                      | Allows for things like {% raw %}`{{ child.description ?: child.content\|striptags\|pretty_truncate(200) }}`{% endraw %}
`su.page.description`                  |
`su.page.featuredImage`                | `.fit(x,y)`, `.fill(x,y)`, `.ratio(x,y)`
`su.page.navigationLabel`              |
`su.page.parent`                       | Parent of current page
`su.page.posts`                        | Get the posts associated with a blog page. Posts have the same content as pages (title, etc).
`su.page.publishedAt`                  | To format in Twig: {% raw %}`{{ su.page.publishedAt|date( "F j, Y" ) }}`{% endraw %}
`su.page.target`                       |
`su.page.thumbnail`                    |
`su.page.title`                        | Does not allow inline editing
`su.page.topParent`                    | Top parent of current page
`su.page.updatedAt`                    | To format in Twig: {% raw %}`{{ su.page.updatedAt|date( "F j, Y" ) }}`{% endraw %}
`su.page.url`                          |
`su.page.urlForCollectionItem( item )` | Retrieve the URL for a collection item's detail page
`su.page.collectionItem()`             | Retrieve current item on collection item detail page

## su.request

Item                    | Description
------------------------|-------------
`su.request.path`       |
`su.request.protocol`   |
`su.request.query`      | Allows access to POST and GET data
`su.request.serverName` |
`su.request.url`        |

## su.site

Item                 | Description
---------------------|--------------
`su.site.address`    |
`su.site.city`       |
`su.site.country`    |
`su.site.logo`       | `.fit(x,y)`, `.fill(x,y)`, `.ratio(x,y)`
`su.site.name`       | Website name
`su.site.navigation` | `item.name`, `item.url`, `item.target`, `item.current`, `item.hasParent`, `item.hasChildren`
`su.site.phone`      |
`su.site.secondline` | Website slogan
`su.site.state`      |
`su.site.url`        | Website's primary URL
`su.site.zip`        |

## su.site.sitemap

Item                   | Description
-----------------------|---------------------------------------------
`su.site.sitemap`      | Array of pages
`page.hasChildren`     | Boolean
`page.hasParent`       | Boolean; usually only false for home page
`page.current`         | Boolean
`page.url`             | String
`page.target`          | Used for anchor target attribute
`page.navigationLabel` | Label specifically for use in navigation

Example usage:

{% raw %}
```twig
<ul>
  {% spaceless %}
    {% for page in sitemap %}
      {% set hasChildren = page.hasChildren and page.hasParent %}
      <li class="{% if page.current %}active{% endif %} {% if hasChildren %}has-children{% endif %}">
        <a href="{{ page.url }}" target="{{ page.target }}">{{ page.navigationLabel }}</a>
        {% if hasChildren %}
          <ul>
            {% for child in page.children %}
              <li{% if child.current %} class="active"{% endif %}><a
                    href="{{ child.url }}" target="{{ page.target }}">{{ child.navigationLabel }}</a></li>
            {% endfor %}
          </ul>
        {% endif %}
      </li>
    {% endfor %}
  {% endspaceless %}
</ul>
```
{% endraw %}

## su.social

Returns an array of social links.

Item          | Description
--------------|----------------------------
`social.url`  | Social link url
`social.name` | Social link label
`social.icon` | Social link icon identifier

Example usage:

{% raw %}
```twig
<ul>
{% for link in su.social %}
  <li><a href="{{ link.url }}" target="_blank">{{ getIcon( link.icon ) }} {{ link.name }}</a></li>
{% endfor %}
</ul>
```
{% endraw %}

## su.theme

Item                                     | Description
-----------------------------------------|--------------------------------
`su.theme.asset( (string) path )`        | Path in theme's `asset/` folder
`su.theme.config( (string) identifier )` | Retrieve theme config value
`su.theme.css( (string) path )`          | Path in theme's `style/` folder
`su.theme.js( (string) path )`           | Path in theme's `script/` folder

## su.user

Item                 | Description
---------------------|--------------
`su.user.first_name` |
`su.user.is_admin`   | Boolean
`su.user.last_name`  |

## Related reading

- [Twig documentation](https://twig.symfony.com/doc/2.x/)
- [Theme Customization](theme_customization.md)
