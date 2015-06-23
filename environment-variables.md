# Environment Variables

Instead of defining database details or API keys in the `wp-config.php` we use environment variables. This means important details can be kept out of the source control with the added benefit of allowing each team member to define their own setup.

To get started lets copy the `.env.example` to `.env`. We'll focus on the top section for now as the rest is covered under their own specific sections. Lets start with database details:

```
DB_NAME=suzie
DB_USER=root
DB_PASSWORD=password
DB_HOST=localhost
DB_PREFIX=wp_
```
You should alter these to match your setup. Now lets move on to app based settings.
```
APP_ENV=local
APP_KEY=6goBCImVkVsv
```
The `APP_ENV` can be used in your app to check which environment your app is running in. An example might be including Usersnap on Staging servers only.

The `APP_KEY` should be updated to your own unique key and can be useful for encrypting data.

Next are a few WordPress settings.

```
WP_DEBUG=true
WP_LOCAL_DEV=true
SITE_URL=http://base.dev
```

`WP_DEBUG` and `WP_LOCAL_DEV` are just your standard WordPress enable debug settings. And `SITE_URL` lets you set the URL so assets and CDNs work correctly.

## Accessing and Defining Variables

At any point in your site you can access varibles using `getenv()`. For example the APP_ENV would be `getenv('APP_ENV')`. Please note `true` and `false` are picked up as strings, so using them with `if` statement should be:

``` php
if (getenv('WP_DEBUG') == 'true')
{
  //do the magic
}
```
If you want to add your own variables just drop them into the `.env`. I might use it define my Typekit account so during development I use my own, but when in production it pulls in the clients account. If a plugin requires you define a license key in the `wp-config.php` you should do it like so:

``` php
define('WPMDB_LICENCE', getenv('WPMDB_LICENCE'));
```
Then in your `.env` add:
```
WPMDB_LICENCE=xxxx-xx-xx-xx-xxxxxx
```
