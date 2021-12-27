# ThemeHandbook -- DO NOT DELETE UNTIL DOCUMENTATION HAS BEEN TRANSFERRED TO SUF WIKI

https://simpleupdates.github.io/ThemeHandbook

Based on the [Primer theme](https://github.com/pages-themes/primer).

## Installation

```bash
gem install bundler jekyll
```

## Usage

Using host:

`bundle exec jekyll serve`

Using Docker:

`docker-compose up`

[http://localhost:4000](http://localhost:4000)

### Docker

Command                                                     | Notes
------------------------------------------------------------|------------------------------------
`docker-compose up -d`                                      | Spin up site
`docker-compose exec site jekyll build --baseurl /`         | Rebuild site
`docker-compose exec site jekyll build --watch --baseurl /` | Watch for changes
`docker logs themehandbook_site_1`                          | View logs
`docker-compose exec site bundle update`                    | Update dependencies
