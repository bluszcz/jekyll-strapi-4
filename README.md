# jekyll-strapi reboot

## Features

* Support for Strapi 4
* Authentication
* Permalinks
* Caching and collecting assets from Strapi

## Install

Add the "jekyll-strapi" gem to your Gemfile:

```
gem "jekyll-strapi"
```

Then add "jekyll-strapi" to your plugins in `_config.yml`:

```
plugins:
    - jekyll-strapi
```

## Configuration

```yaml
strapi:
    # Your API endpoint (optional, default to http://localhost:1337)
    endpoint: http://localhost:1337
    # Collections, key is used to access in the strapi.collections
    # template variable
    collections:
        # Example for a "Photo" collection
        photos:
            # Collection name (optional)
            # type: photos
            # Permalink used to generate the output files (eg. /articles/:id).
            permalink: /photos/:id/
            # Layout file for this collection
            layout: photo.html
            # Generate output files or not (default: false)
            output: true
```

This works for the following collection *Photo* in Strapi:

| Name    | Type  |
| ------- | ----- |
| Title   | Text  |
| Image   | Media |
| Comment | Text  |

### Authentication

To access non Public collections (and by default all Strapi collections are non Public) you must to generate a token inside your strapi instance and set it as enviromental variable `STRAPI_TOKEN`.

## Usage

This plugin provides the `strapi` template variable. This template provides access to the collections defined in the configuration.  

### Using Collections

Collections are accessed by their name in `strapi.collections`. The `articles` collections is available at `strapi.collections.articles`.

To list all documents of the collection ```_layouts/home.html```:

```
---
layout: default
---
<div class="home">
  <h1 class="page-heading">Photos</h1>
  {%- if strapi.collections.photos.size > 0 -%}
  <ul>
    {%- for photo in strapi.collections.photos -%}
    <li>
      <a href="{{ photo.url }}">Title: {{ photo.title }}</a>
    </li>
    {%- endfor -%}
  </ul>
  {%- endif -%}
</div>
```

### Attributes

All object's data you can access through ``` {{ page.document.strapi_attributes }}```. This plugin also introduces new filter asset_url which perform downloading the asset into the assets folder and provides correct url. Thanks for this you have copies of your assets locally without extra dependency on Strapi ```_layouts/photo.html```:

```
---
layout: default
---

<div class="home">
  <h1 class="page-heading">{{ page.document.title }}</h1>
  <h2>{{ page.document.strapi_attributes.Title }}</h2>
  <p>{{ page.document.strapi_attributes.Comment }}</p>
  <img src="{{ page.document.strapi_attributes.Image.data.attributes.formats.thumbnail| asset_url }}"/>
</div>
```
