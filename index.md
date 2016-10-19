# Getting Started

Welcome, Suzie is a starter kit for WordPress. It offers a better way to build scalable WordPress sites by making sure the Wordpress core, assets/media and configuration are not stored in your source control. This enables support for multiple server setups. On top of this you have the options of using Mailgun for emails and built-in Varnish Cache support.

## Requirements

Suzie requires [Composer](https://getcomposer.org/). Please make sure it's installed before continuing.  

#### Installing Composer on Linux or Mac OS X

```
$ curl -sS https://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin/composer
```

#### Installing on Windows

Download the installer from [getcomposer.org/download](getcomposer.org/download), execute it and follow the instructions.

## Installation

Download the latest version of the Suzie and extract its contents into a directory on your server. Next, in the root, run.

```
$ composer install
```

Please ensure your domain points to the `public` folder. Now lets move on to explaining [Composer](/$branch/composer).
