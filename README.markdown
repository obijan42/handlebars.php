Handlebars.php
==============
[![CI](https://github.com/obijan42/handlebars.php/actions/workflows/ci.yml/badge.svg)](https://github.com/obijan42/handlebars.php/actions/workflows/ci.yml)

A maintained fork of the abandoned [`xamin/handlebars.php`](https://github.com/XaminProject/handlebars.php),
updated to run on PHP 7.2 through 8.x. Drop-in compatible: the namespace and public API are unchanged.

> Migrating from `xamin/handlebars.php`? The only breaking change is the removal of the
> long-deprecated `\Handlebars\String` class — use `\Handlebars\StringWrapper` instead.
> Note that default escaping now uses `ENT_QUOTES` (single quotes are escaped too);
> see [CHANGELOG.md](CHANGELOG.md) and [SECURITY.md](SECURITY.md).

> **Alternatives:** [`salesforce/handlebars-php`](https://github.com/salesforce/handlebars-php)
> is another fork (from the `mardix` lineage, PHP 5.4+, last active 2023) with `@data`
> variables and `#each` slice syntax. This fork descends from XaminProject's `develop`
> branch and aims to be a closer drop-in replacement for `xamin/handlebars.php`.

installation
------------

```
$ composer require obijan42/handlebars.php
```

usage
-----

```php
<?php

// uncomment the following two lines, if you don't use composer
// require 'src/Handlebars/Autoloader.php';
// Handlebars\Autoloader::register();

use Handlebars\Handlebars;

$engine = new Handlebars;

echo $engine->render(
    'Planets:<br />{{#each planets}}<h6>{{this}}</h6>{{/each}}',
    array(
        'planets' => array(
            "Mercury",
            "Venus",
            "Earth",
            "Mars"
        )
    )
);
```

```php
<?php

use Handlebars\Handlebars;

$engine = new Handlebars(array(
    'loader' => new \Handlebars\Loader\FilesystemLoader(__DIR__.'/templates/'),
    'partials_loader' => new \Handlebars\Loader\FilesystemLoader(
        __DIR__ . '/templates/',
        array(
            'prefix' => '_'
        )
    )
));

/* templates/main.handlebars:

{{> partial planets}}

*/

/* templates/_partial.handlebars:

{{#each this}}
    <file>{{this}}</file>
{{/each}}

*/

echo $engine->render(
    'main',
    array(
        'planets' => array(
            "Mercury",
            "Venus",
            "Earth",
            "Mars"
        )
    )
);
```

contribution
------------

contributions are more than welcome, just don't forget to:

 * add your name to each file that you edit as author
 * use PHP CodeSniffer to check coding style.

license
-------

    Copyright (c) 2010 Justin Hileman
    Copyright (C) 2012-2013 Xamin Project and contributors

    Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
