# Composer template for Drupal projects

[![Build Status](https://travis-ci.org/drupal-composer/drupal-project.svg?branch=8.x)](https://travis-ci.org/drupal-composer/drupal-project)

This project template provides a starter kit for Drupal 8 vanilla site with Pattern lab and Particle as base theme.

## Build with Lando


First you need to [install composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx).

> Note: The instructions above link refer to the [global composer installation](https://getcomposer.org/doc/00-intro.md#globally).
You might need to replace `composer` with `php composer.phar` (or similar) 
for your setup.

If you dont have docker and lando setup on ur machine, [Follow here](https://docs.devwithlando.io/installation/installing.html) for Lando installation 
Windows user can [follow this guide](https://docs.google.com/document/d/158hEpDepMMHNjv4Rvmq5whB5M9Q_OQNO4Log7bpK7SY/edit)

clone this repository and run `lando composer install` from the root

copy default.settings.php and create settings.php located in `web/sites/default/` and update settings.php  file with local db details(check your lando info to get db credentials).
```
    $databases['default']['default'] = array (
    'database' => 'drupal8',
    'username' => 'drupal8',
    'password' => 'drupal8',
    'prefix' => '',
    'host' => 'database',
    'port' => '3306',
    'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
    'driver' => 'mysql',
  );
  ```
  
Config sync path in seetings.php must be set to 

```$config_directories['sync'] = '../config/sync';```

This is your local env config, so you don't need to commit in repository.


To setup pattern lab particle theme, go to `{root}/web/themes/particle` and run
```
   bash
   npm install
   npm run setup
   npm run build:drupal

```

Go to drupal appearance and enable Particle theme and clear cache


you can run pattern lab using `npm start` while in {root}/web/themes/particle`
On successfull build and start, pattern lab will be avaialble here:

>http://0.0.0.0:8080/app-pl/pl/



## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also 
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Should I commit the scaffolding files?

The [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@composer drupal:scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```
### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull 
request is often a better solution), you can do so with the 
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra 
section of composer.json:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```
### How do I switch from packagist.drupal-composer.org to packages.drupal.org?

Follow the instructions in the [documentation on drupal.org](https://www.drupal.org/docs/develop/using-composer/using-packagesdrupalorg).

### How do I specify a PHP version ?

Currently Drupal 8 supports PHP 5.5.9 as minimum version (see [Drupal 8 PHP requirements](https://www.drupal.org/docs/8/system-requirements/drupal-8-php-requirements)), however it's possible that a `composer update` will upgrade some package that will then require PHP 7+.

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:
```json
"config": {
    "sort-packages": true,
    "platform": {"php": "5.5.9"}
},
```
