# CDN

Its good practise to use a CDN (content delivery network) on your site. It has two advantages:

* Takes the load of your server.
* It's quicker as the assets are sent from a location close to the user.

The general set up for this is a subdomain like `cdn.bigbitecreative.com` that points to a CDN provider. If the file exists the CDN serves the file. Otherwise it will load it from your server.

However this set up won't work for us as we're using S3/SFTP for assets. Therefore we need to use two CDNs, one for the site so any CSS, JS etc. and one for the remote assets on S3/SFTP.

## Defining CDNs

In your `.env` enable the site CDN and provide the URL:

```
CDN_SITE_ENABLED=true
CDN_SITE_URL=https://cdn.bigbitecreative.com
```

Now for the assets CDN:

```
CDN_ASSET_ENABLED=true
CDN_ASSET_URL=https://cdn-assets.bigbitecreative.com
```

## Using a CDN with S3

If you want to use a CDN with S3 you should look into [CloudFront](http://aws.amazon.com/cloudfront/) which lets you point to an S3 bucket. You will then have a CloudFront URL like:

`http://bigbite-xxx.cloudfront.net`

Then in your `.env` you can just update the `CDN_ASSET_URL` with this:

```
CDN_ASSET_URL=http://bigbite-xxx.cloudfront.net/
```
