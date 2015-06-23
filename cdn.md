# CDN

Its good practise to use a CDN (content delivery network) on your site. It has two advantages:

* Takes the load of your server.
* It's quicker as the assets are sent from a location close to the user.

The general set up for this is a subdomain like `cdn.bigbitecreative.com` that points to a CDN provider. If the file exists the CDN serves the file. Otherwise it will load it from your server.

However this set up won't work for us as we're using S3 for assets. Therefore we need a way to move javascript and CSS files over to S3 during deployments. To solve this Suzie has a small task attached to `composer install` which will move any files you have defined in your `wp-config.php` to S3.

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

You need to supply the name of your theme folder. In this case I'll use `bigbite`:

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

If you want to use a CDN with S3 you should look into [CloudFront](http://aws.amazon.com/cloudfront/) which lets you point to an S3 bucket. You will then have a CloudFront URL like:

`http://bigbite-xxx.cloudfront.net`

Then in your `.env` you can just update the CDN_URL with this:

```
CDN_URL=http://bigbite-xxx.cloudfront.net/
```
