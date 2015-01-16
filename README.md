Laravel Menu Builder
--------------------

**A menu builder for Laravel using Bootstrap's markup.**

[![Build Status](https://travis-ci.org/lazychaser/illuminate-menu.png?branch=master)](https://travis-ci.org/lazychaser/illuminate-menu)
[![Latest Stable Version](https://poser.pugx.org/kalnoy/illuminate-menu/version.png)](https://packagist.org/packages/kalnoy/illuminate-menu)
[![Total Downloads](https://poser.pugx.org/kalnoy/illuminate-menu/d/total.png)](https://packagist.org/packages/kalnoy/illuminate-menu)

Installation
============

Install using Composer:

```
composer require kalnoy/illuminate-menu:~1.0
```

Add a service provider:

```php
'providers' => [
    'Illuminate\Html\MenuServiceProvider',
],
```

And a facade:

```php
'aliases' => [
    'Menu' => 'Illuminate\Html\MenuFacade',
],
```

Documentation
=============

Rendering a menu:

```php
{{ Menu::render($items, $attributes) }}
```

Where `$attributes` is optional array of html attributes for `ul` element.

Rendering a single menu item:

```php
{{ Menu::item($options) }}
```

Basic example:

```php
Menu::render([
    'Link to url' => 'bar',
    'Link to external url' => 'http://bar',
    [ 'label' => 'Link to url', 'url' => 'bar' ],
    [ 'label' => 'Link to route', 'route' => [ 'route.name', 'foo' => 'bar' ] ],
]);
```

Rendering an item with a drop down menu:

```php
{{ Menu::item([
    'label' => 'Settings',
    'icon' => 'wrench',
    'items' => [
        'Foo' => 'bar',
        '-', // divider
        [ 'label' => 'Logout', 'route' => 'logout_path' ],
    ],
]) }}
```

Controlling whether the item is visible:

```php
{{ Menu::item([
    'label' => 'Foo',
    'url' => 'bar',
    'visible' => function () { return Config::get('app.debug'); },
] }}
```

#### Item options

You can specify an array of following options:

*   `label` is a label of the item, automatically translated, so you can specify lang string id
*   `url` is the url which can be a full URI or local path
*   `route` to specify a route, possibly with parameters
*   `secure`; specify `true` to make `url` be secure (doesn't affect `route` option)
*   `items` is an array of items for drop down menu

Changing the state of the item:

*   `visible` is a boolean value or closure to specify whether the item is visible
*   `active` is a boolean value or closure to specify whether to add `active` class to item; if not specified, determined
    automatically based on current url
*   `disabled` is a boolean value or closure to specify whether the menu item is disabled

Presentation options:

*   `icon` is a glyphicon id, i.e. `pencil`
*   `badge` is a value for badge (scalar or closure)
*   and any other parameter that will be rendered as an attribute for `<li>` element.

### Customization

Though this menu builder intended to be used together with bootstrap markup, you can customize it however you like by
extending `Illuminate\Html\MenuBuilder` class and overriding base methods.