# Laravel / AdminLTE 3 Boilerplate

[![Packagist](https://img.shields.io/packagist/v/sebastienheyd/boilerplate.svg?style=flat-square)](https://packagist.org/packages/sebastienheyd/boilerplate)
[![Build Status](https://scrutinizer-ci.com/g/sebastienheyd/boilerplate/badges/build.png?b=master&style=flat-square)](https://scrutinizer-ci.com/g/sebastienheyd/boilerplate/build-status/master)
[![StyleCI](https://github.styleci.io/repos/86598046/shield?branch=master&style=flat-square)](https://github.styleci.io/repos/86598046)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/sebastienheyd/boilerplate/badges/quality-score.png?b=master&style=flat-square)](https://scrutinizer-ci.com/g/sebastienheyd/boilerplate/?branch=master)
![Laravel](https://img.shields.io/badge/Laravel-6.x%20→%208.x-green?logo=Laravel&style=flat-square)
[![Nb downloads](https://img.shields.io/packagist/dt/sebastienheyd/boilerplate.svg?style=flat-square)](https://packagist.org/packages/sebastienheyd/boilerplate)
[![MIT License](https://img.shields.io/github/license/sebastienheyd/boilerplate.svg?style=flat-square)](license.md)

This package serves as a basis for quickly creating a back-office. 
It includes profile creation and his management, user management, roles, permissions and log viewing.

It also makes it easy to add other packages to extend the features, have a look to
[sebastienheyd/boilerplate-packager](https://github.com/sebastienheyd/boilerplate-packager) to quickly build your own
package for boilerplate.

Other packages to extend the features :
* [sebastienheyd/boilerplate-media-manager](https://github.com/sebastienheyd/boilerplate-media-manager)
* [sebastienheyd/boilerplate-email-editor](https://github.com/sebastienheyd/boilerplate-email-editor)

## Full documentation

The full documentation is readable on [Github pages](https://sebastienheyd.github.io/boilerplate/)

## Features

* Configurable backend theme [AdminLTE 3](https://adminlte.io/docs/3.0/)
* Css framework [Bootstrap 4](https://getbootstrap.com/)
* Icons by [Font Awesome 5](https://fontawesome.com/)
* Role-based permissions provided by [santigarcor/laratrust](https://github.com/santigarcor/laratrust)
* Forms & Html helpers by [laravelcollective/html](https://github.com/laravelcollective/html)
* Menu dynamically builded by [lavary/laravel-menu](https://github.com/lavary/laravel-menu)
* Menu items activated by [hieu-le/active](https://github.com/letrunghieu/active)
* Server-side datatables methods provided by [yajra/laravel-datatables](https://yajrabox.com/docs/laravel-datatables)
* Image manipulation by [intervention/image](https://github.com/intervention/image)
* Logs visualization by [arcanedev/log-viewer](https://github.com/ARCANEDEV/LogViewer)
* Gravatar import by [thomaswelton/laravel-gravatar](https://github.com/thomaswelton/laravel-gravatar)
* Laravel Blade components for AdminLTE 3
* Javascript session keep-alive
* Localized English / French / Italian / Spanish / Turkish

## Installation

1. In order to install Laravel/AdminLTE Boilerplate run :

```
composer require sebastienheyd/boilerplate
```

2. Run the command below to publish assets, lang and configuration files

```
php artisan vendor:publish --tag=boilerplate
```

3. After you set your database parameters file run :

```
php artisan migrate
```

**Optional**

If you want to quickly test your Laravel application.

```
php artisan serve
```

Now you can point your browser to [http://localhost:8000/admin](http://localhost:8000/admin)

## Configuration

Configuration files can be found in `config/boilerplate` folder.

* [`app.php`](src/config/app.php) : url admin prefix, backend locale, redirection after login (see comments in file), ...
* [`auth.php`](src/config/auth.php) : overriding of `config/auth.php` to use boilerplate models instead
of default Laravel models. Allow you to define if users can register from login page and which role will be assigned
to a new user.
* [`laratrust.php`](src/config/laratrust.php) : overriding of Laratrust (package santigarcor/laratrust)
default config.
* [`menu.php`](src/config/menu.php) : dashboard to use and menu classes
* [`theme.php`](src/config/theme.php) : backend theme configuration

### Dashboard

You can override the dashboard view by publishing it :

```bash
php artisan vendor:publish --tag=boilerplate-dashboard
```

After that, you will find the view `dashboard.blade.php` in `resources/views/vendor/boilerplate`

If you need to have a controller for more flexibility, you can run the following artisan command :

```bash
php artisan boilerplate:dashboard
```

This will publish for you the view, the `DashboardController.php` file in the `app/Http/Controllers/Boilerplate` folder and 
modify the configuration file `config/boilerplate/menu.php`.

You can also use your own dashboard controller by changing the `dashboard` value in `config/boilerplate/menu.php`.

### Adding items to the menu

To add an item to the menu, nothing simpler, use the `artisan boilerplate:menuitem` command provided with boilerplate.

This command will generate the classes needed to generate the menu item in the `app/Menu` folder.

```bash
php artisan boilerplate:menuitem {name} {-s} {-o=100}
```

| option / argument | description |
|---|---|
| name | Class name to generate |
| -s --submenu | Menu item must have sub item(s) |
| -o --order | Menu item order in the backend menu |

Once generated, the files can be edited to customize the item.

You can also add your own providers by adding their classnames to the array of providers in the configuration file
`config/boilerplate/menu.php`. This can be useful if you don't want to use the default directory `app/Menu` in your 
application.

For package developers, menu items providers can be added by using the `boilerplate.menu.items` singleton in your 
package service provider. Example : 

```php
public function boot()
{
    app('boilerplate.menu.items')->registerMenuItem([
        Users::class,
        Logs::class,
    ]);
}
```

For more information, see the documentation of the following packages:

- [lavary/laravel-menu](https://github.com/lavary/laravel-menu)
- [hieu-le/active](https://github.com/letrunghieu/active)

### Loading plugins assets

By default, only jQuery, Bootstrap, Font Awesome and AdminLTE scripts and styles are loaded.

To load and use plugins like Datatables, Date Picker, TinyMCE, ... you can use "loaders". These are blade templates
prepared to add the loading of scripts and styles for a plugin.

For example, you want to use a datepicker on a text field :

```blade
@include('boilerplate::load.datepicker')

@push('js')
    <script>
        $('.datepicker').datepicker();
    </script>
@endpush
```

Here `@include('boilerplate::load.datepicker')` will load scripts and styles to allow usage of datepicker. After that
you can push your scripts on the `js` stack (or styles on the `css` stack).

Available loaders are :

| Loader  | Documentation  |  |
|---|---|---|
| [`boilerplate::load.tinymce`](src/resources/views/load/tinymce.blade.php) | [TinyMCE](https://www.tiny.cloud)  | [Example](src/resources/views/plugins/demo/tinymce.blade.php) |
| [`boilerplate::load.codemirror`](src/resources/views/load/codemirror.blade.php) | [CodeMirror](https://codemirror.net/) | [Example](src/resources/views/plugins/demo/codemirror.blade.php) |
| [`boilerplate::load.datatables`](src/resources/views/load/datatables.blade.php) | [Datatables](https://www.datatables.net/) | [Example](src/resources/views/plugins/demo/datatables.blade.php) |
| [`boilerplate::load.datepicker`](src/resources/views/load/datepicker.blade.php) | [Tempus Dominus](https://tempusdominus.github.io/bootstrap-4/) / [Date Range Picker](https://www.daterangepicker.com) | [Example](src/resources/views/plugins/demo/datepicker.blade.php) |
| [`boilerplate::load.select2`](src/resources/views/load/select2.blade.php) | [Select2](https://select2.github.io/) | [Example](src/resources/views/plugins/demo/select2.blade.php) |
| [`boilerplate::load.fileinput`](src/resources/views/load/fileinput.blade.php) | [Bootstrap FileInput](http://plugins.krajee.com/file-input) | [Example](src/resources/views/plugins/demo/fileinput.blade.php) | 
| [`boilerplate::load.fullcalendar`](src/resources/views/load/fullcalendar.blade.php) | [FullCalendar](https://fullcalendar.io/) | [Example](src/resources/views/plugins/demo/fullcalendar.blade.php) |  
| [`boilerplate::load.moment`](src/resources/views/load/moment.blade.php) | [MomentJs](http://momentjs.com/) | |

Some plugins are loaded by default :

* [Bootbox](https://github.com/makeusabrew/bootbox) -
[Example](src/resources/views/plugins/demo/bootbox.blade.php)
* [Toastr](https://codeseven.github.io/toastr/) -
[Example](src/resources/views/plugins/demo/notify.blade.php)
* [iCheck](https://github.com/bantikyan/icheck-bootstrap) - [Example](src/resources/views/plugins/demo/icheck.blade.php)

You can see examples on the default dashboard.

### Language

By default the language used by boilerplate is the application language declared into `config/app.php` (locale). You can
define another language only for the back-office by setting `locale` parameter in `config/boilerplate/app.php`.  
Supported language are English, French, Italian, Spanish and Turkish.

When you run `php artisan vendor:publish --tag=boilerplate`, only the language files for form validation are copied 
for supported languages. Thanks to [Laravel-Lang/lang](https://github.com/Laravel-Lang/lang) package !

You can translate or change translations by running `php artisan vendor:publish --tag=boilerplate-lang`. After running 
this command, you will find translations folders into `resources/lang/vendor/boilerplate`. Copy one of the language 
folders in the new language you want to create and all you have to do is to translate. If you want to share the 
language you have added, don't hesitate to make a pull request.

### Routes

Routes are loaded from the file [`boilerplate.php`](src/routes/boilerplate.php).

A default prefix `admin` is set into the config file [`app.php`](src/config/app.php), this is why
boilerplate is accessible by /admin url. You can set an empty prefix if you remove the default route / defined in
`routes/web.php`

### Views

You can override views by running `php artisan vendor:publish --tag=boilerplate-views`. You will then find the views 
in the `resources/views/vendor/boilerplate` folder.

## Package update

Boilerplate comes with assets such as Javascript, CSS, and images. Since you typically will need to overwrite the assets
every time the package is updated, you may use the ```--force``` flag :

```
php artisan vendor:publish --tag=boilerplate-public --force
```

To auto update assets each time package is updated, you can add this command to `post-autoload-dump` into the
file `composer.json` at the root of your project.

```json
{
    "scripts": {
        "post-autoload-dump": [
            "@php artisan vendor:publish --tag=boilerplate-public --force -q"
        ]
    }
}
```

## Tests / Coding standards

This package is delivered with a `Makefile` used to launch checks for the respect of coding standards and the unit tests

Just call `make` to see the list of commands.

### Laravel Dusk functionnal tests

This package is also delivered with functional tests using [Laravel Dusk](https://laravel.com/docs/dusk)

After installing Laravel, Laravel Dusk and configuring your database, you can start the tests with the following command :

```
php artisan dusk vendor/sebastienheyd/boilerplate/tests/DuskTest.php
```

**Important** : Never launch tests with Laravel Dusk if you have data in your database,  Dusk will wipeout all your datas

## Troubleshooting

### Dates localization

Since Laravel 5.8, this package use [Carbon 2](https://carbon.nesbot.com/docs/#api-localization) instead of [Jenssegers/Date](https://github.com/jenssegers/date) to translate dates.

Date format now use the format of momentjs. To translate your dates, you must now use the Carbon 2 class method `isoFormat`
instead of `format`

See [Carbon 2 documentation](https://carbon.nesbot.com/docs/#api-localization)

### Migration error

MySQL < v5.7.7 or MariaDB

When you run `php artisan migrate` and you hit this error :

```
[PDOException]
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes
```

This is an error from a change since Laravel 5.4 : [see here](https://laravel-news.com/laravel-5-4-key-too-long-error)

To correct this, two possibilities :

- Define `utf8` instead of `utf8mb4` as default database charset and `utf8_unicode_ci` instead of
`utf8mb4_unicode_ci` as default database collation.

- Edit your `app/Providers/AppServiceProvider.php` file and define a default string inside the boot method :

```php
use Illuminate\Support\Facades\Schema;

public function boot()
{
    Schema::defaultStringLength(191);
}
```

## Contributing

Please see [contributing.md](contributing.md) for details and a todolist.

## Credits

- [Sébastien HEYD](https://github.com/sebastienheyd)
- [All Contributors](https://github.com/sebastienheyd/boilerplate/contributors)


## License

This package is free software distributed under the terms of the [MIT license](license.md).

## Special thanks

This project is made with [PhpStorm](https://www.jetbrains.com/phpstorm/) and supported by [JetBrains](https://www.jetbrains.com/?from=LaravelBoilerplate)

[![JetBrains Logo](jetbrains.svg)](https://www.jetbrains.com/?from=LaravelBoilerplate)