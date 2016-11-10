# Laravel from Scratch

Part of what makes Nanobox so useful is you don't have to have PHP, Apache, etc., installed on your local machine to run Laravel apps. This guide walks through creating a simple Laravel app from scratch with Nanobox.

The process outlined here is the same process used to create the [nanobox-laravel](https://github.com/nanobox-quickstarts/nanobox-laravel) quickstart found under [nanobox-quickstarts](https://github.com/nanobox-quickstarts) on Github.

*If you have an existing Laravel project, the [Existing Laravel App guide](/php/laravel/existing-app) is where you should start.*


## Create a PHP Dev Environment
Nanobox will create an isolated virtual environment and mount your local codebase inside it. From within this environment you can run the app, artisan commands, or other tasks as you would normally.

Create a new project directory and cd into it.

```bash
# create a new project directory
mkdir nanobox-laravel

# navigate into the new directory
cd nanobox-laravel
```

### Add a boxfile.yml
The [boxfile.yml](https://docs.nanobox.io/boxfile/) tells Nanobox how to build and configure your environment. Create a `boxfile.yml` at the root of your project that contains the following:

```yaml
code.build:

  # tells nanobox to install php and associated runtimes
  engine: php
  config:

    # sets the php version to 7.0
    runtime: php-7.0

    # specifies the webserver document_root
    document_root: public

    # enables php extensions
    extensions:

      # required by laravel
      - pdo
      - mbstring
      - tokenizer
      - session

      # required by composer
      - phar
      - filter
      - json
      - hash
      - zip
      - dom
      - xml

# creates a web component in sim and production environments
web.laravel:

  # commands to start PHP-FPM and Apache
  start:
    fpm: start-php
    apache: start-apache

  # pipes log output in your app's log-stream
  log_watch:
    apache[access]: /data/var/log/apache/access.log
    apache[error]: /data/var/log/apache/error.log
    php[error]: /data/var/log/php/php_error.log
    php[fpm]: /data/var/log/php/php_fpm.log
    laravel[error]: /app/storage/logs/laravel.log
```

### Build the Environment
With the your boxfile.yml in place, you're ready to create your development (dev) environment. From your project directory, run:

```bash
# start the dev environment
nanobox dev start

# add a convenient way to access your app from the browser
nanobox dev dns add laravel.nanobox.dev
```

## Create a New Laravel App
With your dev environment running, you can console into it and install Laravel.

```bash
# console into the dev environment
nanobox dev console

# download the laravel installer
composer global require "laravel/installer"

# create a new laravel app
laravel new
```

## Start PHP-FPM & Apache
Either `exit` out of your dev console or open a new terminal window and run the following to start PHP-FPM and Apache.

```bash
# run the start commands specified in your boxfile.yml
nanobox dev run
```

### View the App in Your Browser
With your app running, you can access it at `laravel.nanobox.dev` in your browser of choice.

## Now What?
Now that you have Laravel running on Nanobox, what's next? Think about what else your application might need and hopefully the topics below will help you get started with the next steps of your development!

* Connecting to a database
* Adding components
* Preparing for production
* Launching your app