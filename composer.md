# Composer

As mentioned in the Getting Started we can manage the WordPress core through composer. If you open up the `composer.json` you will see the `name` section.

``` json
"name": "repo/name",
```
This should be updated to your project like so:
``` json
"name": "jasonagnew/my-new-wordpress-site",
```
If you move down a little instead the `composer.json` you will see the `require` section which is where we can set a WordPress version.
``` json
"require": {
   "johnpbloch/wordpress": "4.2.*"
 }
```
You can change this to the specific version you need or leave it as is. For this example lets say `4.2.2` broke some compatibility so I required an earlier version I could checkout which versions are available:

https://github.com/johnpbloch/wordpress/releases

Then update my `composer.json` to:
``` json
"require": {
   "johnpbloch/wordpress": "4.1.4"
 }
```
And then run:
```
$ composer install
```

## Managing plugins with composer

You can also manage your plugins with composer. First to find a list of plugins we visit:

http://wpackagist.org/

Lets add Advanced Custom Fields, open the `
composer.json` again and visit the `require` section. and at the bottom add:
```json
"wpackagist-plugin/advanced-custom-fields": "4.4.2"
```
So it should like so:
``` json
"require": {
   "johnpbloch/wordpress": "4.1.4",
   ....
   "wpackagist-plugin/advanced-custom-fields": "4.4.2"
 }
```
And then run:
```
$ composer install
```
If you plan to use composer for plugins you will want to exclude them from git. Lets do that by opening your `.gitignore` and go the `WordPress` section.

```
# WordPress
public/wordpress
public/content/uploads/*
.env
```
And update it to:
```
# WordPress
public/wordpress
public/content/uploads/*
public/content/plugins/*
.env
```
