# CDN

Its good practise to use a CDN (content delivery network) on your site. It has two advantages:

* Takes the load of your server.
* Quicker for the user as the assets sent from a location close to the user.

The general set up for this a subdomain like `cdn.bigbitecreative.com` points to a cdn provider. If the file exists the cdn serves the file otherwise it'll load it from your server.

However this set up won't work for us as we use S3 for assets. Therefore we need a way to move javascript and css files over to S3 during deployments so Suzie has a small task attached to `composer install` which will move any files you have defined in your `wp-config.php` to S3.

## Defining Files

Open your `wp-config.php` and near the bottom:

``` php
/**
 * Suzie
 * use {theme} or {plugin}
 */
define('SUZIE_CDN_THEME', 'bbwp');
define('SUZIE_CDN_ASSETS', json_encode([

]));
```

You need to supply the name of your theme folder, in this case I'll use `bigbite`:

``` php
define('SUZIE_CDN_THEME', 'bigbite');
```

Then `SUZIE_CDN_ASSETS` takes an array of items to upload. Using `{theme}` to link to the theme folder and `{plugin}` to link to the plugin folder:

``` php
define('SUZIE_CDN_THEME', 'bigbite');
define('SUZIE_CDN_ASSETS', json_encode([
  '{theme}/assets/js/app.js',
  '{theme}/assets/css/app.css',
  '{theme}/assets/css/app.css',
  '{theme}/assets/font/font-name.eot',
  '{plugin}/contact-form-7/includes/css/styles.css'
]));
```

Then in your `.env` enable the cdn:

```
CDN_ENABLED=true
```

## Using a CDN with S3

If you want to use a CDN with S3 you should look into [CloudFront](http://aws.amazon.com/cloudfront/) which lets you point to an S3 bucket. You will then have CloudFront url like:

`http://bigbite-xxx.cloudfront.net`

Then in your `.env` you can just update the CDN_URL with this:

```
CDN_URL=http://bigbite-xxx.cloudfront.net/
```
