---
title: Twig API
permalink: /twig-api/
---

## General

Item                   | Description
-----------------------|--------------
`su.calendar.events`   | `event.id`, `event.title`, `event.description`, `event.start`, `event.end`, `event.location`, `event.isAllDay`
`su.content`           |
`su.footer`            |
`su.misc.privatelabel` | Retrieve site's private label
`.fit(x,y)`            | Resizes image down to fit the given dimensions. Does not upscale.
`.fill(x,y)`           | Crops image to fit the given dimensions. Does not crop if the image is already smaller than the given dimensions.
`.ratio(x,y)`          | Always crops image to match the ratio of the given dimensions, regardless of image's resolution.

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

Example usage:

{% raw %}
```twig
{% set button_items = su.collection( "gumwoodButtons" )
		.activationDate( "now"|date( 'Y-m-d' ), "<=" )
		.deactivationDate( "now"|date( 'Y-m-d' ), ">" ).or().deactivationDate( "", "=" )
		.find() %}
```
{% endraw %}

## su.editable

Item                                        | Description
--------------------------------------------|--------------------------------------------------
`su.editable.region( (string) identifier )` | Allows inline editing of a custom content region
`su.editable.title`                         | Allows inline editing of page title

Editable region identifer strings are most comonly named `custom1`, `custom2`, `custom3`, etc. `custom1` almost always coincides with a layout's sidebar. Whenever possible, stick to this naming convention so that a user's page content remains visible when they switch to a layout with a sidebar in another theme.

## su.page

Item                      | Description
--------------------------|-------------------------------         
`su.page("/url")`         | Get page by path
`su.page.authors`         |
`su.page.breadcrumbs`     |
`su.page.children`        | Get a page's children
`su.page.children(id)`    | Get the children of a specific page. Inspect the table row in the admin page list to get the id.
`su.page.content`         | Allows for things like {% raw %}`{{ child.description ?: child.content\|striptags\|pretty_truncate(200) }}`{% endraw %}
`su.page.description`     |
`su.page.featuredImage`   | `.fit(x,y)`, `.fill(x,y)`, `.ratio(x,y)`
`su.page.navigationLabel` |
`su.page.parent`          | Parent of current page
`su.page.posts`           | Get the posts associated with a blog page. Posts have the same content as pages (title, etc).
`su.page.publishedAt`     | To format in Twig: {% raw %}`{{ su.page.publishedAt|date( "F j, Y" ) }}`{% endraw %}
`su.page.target`          |
`su.page.thumbnail`       |
`su.page.title`           | Does not allow inline editing
`su.page.topParent`       | Top parent of current page
`su.page.updatedAt`       | To format in Twig: {% raw %}`{{ su.page.updatedAt|date( "F j, Y" ) }}`{% endraw %}
`su.page.url`             |

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

```twig
<ul>
{% for link in su.social %}
  <li><a href="{{ link.url }}" target="_blank">{{ getIcon( link.icon ) }} {{ link.name }}</a></li>
{% endfor %}
</ul>
```

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
