Guetzli Documentation
=====================

Guetzli, a deliciously fast and simple Filesystem-Web-CMS. For setup and design, please see the [README](../README.md).

The Biscuit
-----------
Dynamic content (i.e. the 'bisc') is the backbone of Guetzli. It's generated using the pages and posts under `./content` and the templates under `./design`. To your visitors it is served using the following URLs:

`/`

The root landing page matches the visitor's language to the available ones listed in `content/config.json` --> `active_languages` and serves the page defined as default in `content/config.json` --> `default_pagename`, rendered using `design/template.html`. In case there is no language match it uses `content/config.json` --> `default_language`.

`/bisc`

Same as root.

`/bisc/[language]`

Same as root, but serves a specific language.

`/bisc/[language]/[pagename]`

Serves a specific page if available under `content/pages/[language]/[pagename].html` using `design/template.html`. If unavailable, serves `default_pagename` in the chosen language.

`/bisc/[language]/[post-type]/[post-id]`

Serves a specific post if available under `content/posts/[post-type]/[language]/[post-id].html` using `design/template.html`. If unavailable, serves `default_pagename` in the chosen language.

The Chocolate
-------------
HTML pages are fine, but they may be a bit boring and dry to some visitors. To make your Guetzli more tasty, add some static files like CSS, images, javascript or whatever you like under `./design/static/[your-folder-structure]`. This is served as `/choco/[your-folder-structure]`.

HowTo
-----
*Add/Change/Remove a (blog-) post*

Add/Change/Remove a file under `content/posts/[post-type]/[language]` using a `YYYY-MM-DD-[AuthorShortname]-[AnyTitle].html` filename format. See the existing examples. Specially available tags for blog entries are `{{ author }}` `{{ post_path }}` (URL to the standalone entry) and  `{{ publishing_date }}`.

*Add/Change/Remove a page*

Add/Change/Remove a file under `content/pages/[language]`. From the moment the file exists, the page is reachable under the URL `/[language]/[filename(without .html)]`.

*Link a page in the menu*

Add `{"title": "[Given Title]", "name":[filename(without .html)]}` under the correct language list in `content/config.json` --> `pages_by_language`.

*Add a new localized string*

Add it in `content/config.json` --> `strings_by_template_reference` using the reference as key (see examples) and use it with `{{ reference }}` in any page, post or template.

*Change the appearance*

Edit `design/template.html`.

*Add a new post type*

1) Add a new directory with language subdirectories under `content/posts`.

2) Copy `design/blog-items` as `design/[post-dir-name]-items`. Adapt it to your needs.

3) Add the `{{ [post-dir-name]_items }}` tag on a page where you'd like to list your posts.

4) Reference the posts directory in `content/config.json` --> `active_post_types_by_pagename` (see the blog as an example).

*example content/config.json*
```
{
  "pages_by_language": {
    "en": [
      {"title": "About Protogrid", "name": "001-about"},
      {"title": "Blog", "name": "002-blog"},
      {"title": "Events", "name": "003-events"}
    ],
    "de": [
      {"title": "Über Protogrid", "name": "001-about"},
      {"title": "Blog", "name": "002-blog"},
      {"title": "Ereignisse", "name": "003-events"},
      {"title": "Speziell für Deutschsprechende", "name": "00Z-special"}
    ]
  },
  "active_languages": [
    {"language": "English", "id":"en"},
    {"language": "German", "id":"de"},
    {"language": "Spanish", "id":"es"}
  ],
  "strings_by_template_reference": {
    "date_particle": {"en": "on", "de": "am"},
    "page_localized": {"en": "page", "de": "Seite"},
    "of_localized": {"en": "of", "de": "von"},
    "blog_posts_localized": {"en": "blog posts", "de": "Blog-Einträge"},
    "to_localized": {"en": "to", "de": "bis"}
  },
  "active_post_types_by_pagename": {
    "002-blog": [
      {"posts_directory": "blog", "items_per_page": 3}
    ]
  },
  "authors_by_shortname": {
    "mmu": "Michel Müller",
    "msc": "Mark Schmitz"
  },
  "default_pagename": "001-about",
  "default_language": "en"
}
```