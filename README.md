# Add preload and prefetch links based your Mix manifest

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-mix-preload.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-mix-preload)
[![Build Status](https://img.shields.io/travis/spatie/laravel-mix-preload/master.svg?style=flat-square)](https://travis-ci.org/spatie/laravel-mix-preload)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/laravel-mix-preload.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/laravel-mix-preload)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-mix-preload.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-mix-preload)

```blade
<head>
    <title>Preloading things</title>

    @preload
</head>
```

This package exposes a `@preload` Blade directive that renders preload and prefetch links based on the contents in `mix-manifest.json`. Declaring what should be preloaded or prefetched is simple, just make sure `preload` or `prefetch` is part of the chunk name.

If this is your mix manifest:

```json
{
    "/js/app.js": "/js/app.js",
    "/css/app.css": "/css/app.css",
    "/css/prefetch-otherpagecss.css": "/css/prefetch-otherpagecss.css",
    "/js/preload-biglibrary.js": "/js/preload-biglibrary.js",
    "/js/vendors~preload-biglibrary.js": "/js/vendors~preload-biglibrary.js"
}
```

The following links will be rendered:

```html
<link rel="prefetch" href="/css/prefetch-otherpagecss.css" as="style">
<link rel="preload" href="/js/preload-biglibrary.js" as="script">
<link rel="preload" href="/js/vendors~preload-biglibrary.js" as="script">
```

Not sure what this is about? Read Addy Osmani's article [Preload, Prefetch And Priorities in Chrome](https://medium.com/reloading/preload-prefetch-and-priorities-in-chrome-776165961bbf).

## Support us

Learn how to create a package like this one, by watching our premium video course:

[![Laravel Package training](https://spatie.be/github/package-training.jpg)](https://laravelpackage.training)

We invest a lot of resources into creating [best in class open source packages](https://spatie.be/open-source). You can support us by [buying one of our paid products](https://spatie.be/open-source/support-us).

We highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using. You'll find our address on [our contact page](https://spatie.be/about-us). We publish all received postcards on [our virtual postcard wall](https://spatie.be/open-source/postcards).

## Installation

You can install the package via composer:

```bash
composer require spatie/laravel-mix-preload
```

## Usage

Add a `@preload` directive to your applications layout file(s).

```blade
<!doctype html>
<html>
    <head>
        ...
        @preload
    </head>
    <body>
        ...
    </body>
</html>
```

You can determine which scripts need to be preloaded or prefetched by making sure `preload` or `prefetch` is part of their file names. You can set the file name by creating a new entry in Mix, or by using dynamic imports.

### Adding a second entry

By default, Laravel sets up Mix with a single `app.js` entry. If you have another script outside of `app.js` that you want to have preloaded, make sure `preload` is part of the entry name.

```js
mix
    .js('resources/js/app.js', 'public/js');
    .js('resources/js/preload-maps.js', 'public/js');
```

If you want to prefetch the script instead, make sure `prefetch` is part of the entry name.

```js
mix
    .js('resources/js/app.js', 'public/js');
    .js('resources/js/prefetch-maps.js', 'public/js');
```

### Using dynamic imports with custom chunk names

If you want to preload a chunk of your application scripts, make sure `preload` is part of the chunk name. You can use Webpack's magic `webpackChunkName` comment to set the module's chunk name.

```js
import('./maps' /* webpackChunkName: "preload-maps" */).then(maps => {
    maps.init();
});
```

The same applies to prefetching.

```js
import('./maps' /* webpackChunkName: "prefetch-maps" */).then(maps => {
    maps.init();
});
```

### Specifying a custom preload URL

If you wish to preload links from a different URL than your application, you may specify that URL as the second parameter to the Blade `preload` directive:

```
@preload(public_path('mix-manifest.json'), 'https://cdn.example.com')
```

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Postcardware

You're free to use this package, but if it makes it to your production environment we highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using.

Our address is: Spatie, Kruikstraat 22, 2018 Antwerp, Belgium.

We publish all received postcards [on our company website](https://spatie.be/en/opensource/postcards).

## Credits

- [Sebastian De Deyne](https://github.com/sebastiandedeyne)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
