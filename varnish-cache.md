# Varnish Cache

Varnish is great way to speed up your site. It is actually super simple to set up and Suzie will clear your cache any time a post/page is added, updated or deleted. To use Varnish please open `.env`

```
VARNISH_PATH=http://domain.com/.*
VARNISH_ENABLED=true
```

On your server please use the following Varnish config:

```
vcl 4.0;

backend default {
    .host = "127.0.0.1";
    .port = "8080";
}

# Only allow purges from localhost
acl purge {
  # ACL we'll use later to allow purges
  "localhost";
  "127.0.0.1";
  "::1";
}


# Drop any cookies sent to Wordpress.
sub vcl_recv {
  if (req.method == "PURGE") {
    if (req.http.X-Purge-Method == "regex") {
      ban("req.url ~ " + req.url + " && req.    http.host ~ " + req.http.host);
      return (synth(200, "Banned."));
    } else {
      return (purge);
    }
  }
  if (!(req.url ~ "(preview=true|wp-login|wp-admin)")) {
    unset req.http.cookie;
  }
}

# Drop any cookies Wordpress tries to send back to the client.
# Set cache to last 24 hours if Cache-Control HTTP header not set.
sub vcl_backend_response {
  if (beresp.ttl == 120s) {
    set beresp.ttl = 24h;
  }

  if (!(bereq.url ~ "(preview=true|wp-login|wp-admin)")) {
    unset beresp.http.set-cookie;
  }
}
```

## Installing Varnish

If you're not sure how to install Varnish, below is a guide of using it with either Apache or Nginx on Ubuntu 14. To start SSH into your server and run the following five commands.

```
apt-get install apt-transport-https
```
```
curl https://repo.varnish-cache.org/GPG-key.txt | apt-key add -
```
```
echo "deb https://repo.varnish-cache.org/ubuntu/ trusty varnish-4.0" >> /etc/apt/sources.list.d/varnish-cache.list
```
```
apt-get update
```
```
apt-get install varnish
```

### Configuring Varnish

Now we need to configure to run on port 80. Run this to open main config file:

```
sudo nano /etc/default/varnish
```
Find this section and update `6081` to `80` so it looks like below.
```
DAEMON_OPTS="-a :80 \
            -T localhost:6082 \
            -f /etc/varnish/default.vcl \
            -S /etc/varnish/secret \
            -s malloc,256m"
```
Now lets open the config file that tells Varnish how to handle the request.

```
sudo nano /etc/varnish/default.vcl
```
Make this file match the config at the start of this section.


### Working with Apache

We now need to tell apache to run port 8080. Let's open `ports.conf`
```
sudo nano /etc/apache2/ports.conf
```
And update this line to match below.
```
Listen 8080
```
You'll also need to update the `000-default` and your site config.
```
sudo nano /etc/apache2/sites-available/000-default.conf
sudo nano /etc/apache2/sites-available/my-site.conf
```
Changing the top to this:
```
 <VirtualHost *:8080>
```

And finally restart.
```
sudo service apache2 restart
sudo service varnish restart
```

### Working with Nginx

You'll also need to update the `default` and your site config.
```
sudo nano /etc/nginx/sites-available/default
sudo nano /etc/nginx/sites-available/my-site
```
Changing the top to this:
```
server {
     listen 8080;
```

And finally restart.
```
sudo service nginx restart
sudo service varnish restart
```

### Checking if its worked.

You can tell if the site is caching correctly by looking at `X-Varnish` HTTP header.

Cached header has two numbers:
```
X-Varnish:65547 3
```

Not Cached header has one number:
```
X-Varnish:16
```
