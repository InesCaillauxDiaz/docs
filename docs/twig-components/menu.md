---
title: Menu
---
Menu
====

You can create menu's in Bolt by defining them in `menu.yaml`, and subsequently outputting them using `{{ menu() }}`.

Example menu:

```yaml
main:
  - label: Home
    title: This is the <b>first<b> menu item.
    link: homepage
    class: homepage
  - label: About
    link: blocks/about
    submenu:
        - label: Sub 1
          link: entry/29
        - label: Sub 2
          class: menu-item-class
          link: entry/30
        - label: Sub 4
          class: sub-class
          link: page/32
  - label: All pages
    link: pages/
  - label: The Bolt site
    link: https://bolt.cm
    class: bolt-site
    title: Visit the excellent Bolt website!
```

Each menu item has a few attributes you can use to define the menu.

| Attribute      | Description |
|----------------|-------------|
| `label`         | The label of the menu item to generate. |
| `link`     | The link for this menu item. The link can either be an identifier for a single record (like `page/about-us`), a local link (like `pages/` or `search`) or an external link (like `https://boltcms.io`)  |
| `class `       | An optional `class` parameter passed to the template. |
| `title` | The `title` attribute of the menu item, that's shown when the user hovers over the specific menu item. If omitted, the title of a referenced Record will be used |

### Usage in templates

The menu tag renders HTML containing your site's menu items, as defined in the
`menu.yaml` file. There are a few parameters to define the output:

| Argument       | Description |
|----------------|-------------|
| `name`         | The name of the menu to generate. If `null` or omitted, Bolt will build the first menu defined in `menu.yaml`. |
| `template`     | The relative path to the template used to generate the menu. The template must be located under `/templates`. Default value is `"helpers/_menu.html.twig"`  |
| `class `       | An optional `class` parameter passed to the template. |
| `withsubmenus` | When `true`, sub-menus will be included. Default is `true`. |

The default output is taken from the file
`vendor/bolt/core/templates/helpers/_menu.html.twig`. Most often, you'll want
to customize thie file yourself. You can copy this file to your own theme in
`public/themes`, and use the `template` parameter to ensure it's being used.

Examples:

```twig
{{ menu() }}
```

The second example renders the `main` menu, as defined in `menu.yaml`, using
the `partials/_sub_menu.twig`. Note that this file should be present in your
`public/theme/yourthemename` folder.

```twig
{{ menu(name='main', template = 'partials/_sub_menu.twig') }}
```

### menu_array()

Returns an array of the menu items defined in the `menu.yaml` file for the
given `name`. If `name` is `null`, the array contains the definition for the
first menu in the file. The menu_array function is used internally as part of
the previous `menu()` function.

```twig
{{ dump(menu_array('main')) }}
```

Example output of `menu_array()`

```twig
array:4 [▼
  0 => array:7 [▼
    "label" => "Home"
    "title" => "This is the <b>first<b> menu item."
    "link" => "homepage"
    "class" => "homepage"
    "submenu" => null
    "uri" => "/"
    "current" => false
  ]
  1 => array:7 [▼
    "label" => "About"
    "title" => "About This Site"
    "link" => "blocks/about"
    "class" => ""
    "submenu" => array:1 [▼
      0 => array:7 [▼
        "label" => "Sub 1"
        "title" => "Incidunt exercitationem sed."
        "link" => "entry/29"
        "class" => ""
        "submenu" => null
        "uri" => "/entry/incidunt-exercitationem-sed"
        "current" => false
      ]
    ]
    "uri" => "/block/about"
    "current" => false
  ]
  ...
]
```

### admin_menu_array()

Returns an array similar to the menu_array() function, but for the admin menu
instead.