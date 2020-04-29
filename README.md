# Docker for Laravel 7 with PHP-FPM 7.3 & Nginx 1.16 on Alpine Linux
This is fork of [docker-php-nginx](https://github.com/TrafeX/docker-php-nginx) repo
with some changes for supporting Laravel 7 framework, it includes all required PHP libs and composer.
PHP-FPM 7.3 & Nginx 1.16 setup for Docker, build on [Alpine Linux](http://www.alpinelinux.org).
The image is only +/- 43MB large.

GitHub repo: https://github.com/mikhailusov/laravel7-docker
Forked from: https://github.com/TrafeX/docker-php-nginx

* Built on the lightweight and secure Alpine Linux distribution
* Very small Docker image size (+/-35MB)
* Uses PHP 7.3 for better performance, lower cpu usage & memory footprint
* Optimized for 100 concurrent users
* Optimized to only use resources when there's traffic (by using PHP-FPM's ondemand PM)
* The servers Nginx, PHP-FPM and supervisord run under a non-privileged user (nobody) to make it more secure
* The logs of all the services are redirected to the output of the Docker container (visible with `docker logs -f <container name>`)
* Follows the KISS principle (Keep It Simple, Stupid) to make it easy to understand and adjust the image to your needs
* Ready to be used for Laravel 7 deployments


[![Docker Pulls](https://img.shields.io/docker/pulls/musovx/laravel7-docker.svg)](https://hub.docker.com/r/musovx/laravel7-docker)
[![Docker image layers](https://images.microbadger.com/badges/image/musovx/laravel7-docker.svg)](https://microbadger.com/images/musovx/laravel7-docker)
![nginx 1.16.1](https://img.shields.io/badge/nginx-1.16-brightgreen.svg)
![php 7.3](https://img.shields.io/badge/php-7.3-brightgreen.svg)
![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)

## Usage

Clone Laravel framework or navigate to your framework root path:

    $ git clone https://github.com/laravel/laravel.git
    $ cd laravel

Create Dockerfile with following contents:

```
FROM musovx/laravel7-docker:latest

# Copy sources
COPY --chown=nobody . /var/www/html/

# Run composer install to install the dependencies
RUN composer install --optimize-autoloader --no-interaction  --no-progress

```

Build docker image:

    $ docker build -t laravel7-docker .

Run docker image:    

    $ docker run -p 80:8080 laravel7-docker

See the PHP info on http://localhost

Or mount your own code to be served by PHP-FPM & Nginx

    docker run -p 80:8080 -v ~/my-codebase:/var/www/html musovx/laravel7-docker

## Configuration
In [config/](config/) you'll find the default configuration files for Nginx, PHP and PHP-FPM.
If you want to extend or customize that you can do so by mounting a configuration file in the correct folder;

Nginx configuration:

    docker run -v "`pwd`/nginx-server.conf:/etc/nginx/conf.d/server.conf" musovx/laravel7-docker

PHP configuration:

    docker run -v "`pwd`/php-setting.ini:/etc/php7/conf.d/settings.ini" musovx/laravel7-docker

PHP-FPM configuration:

    docker run -v "`pwd`/php-fpm-settings.conf:/etc/php7/php-fpm.d/server.conf" musovx/laravel7-docker

_Note; Because `-v` requires an absolute path I've added `pwd` in the example to return the absolute path to the current directory_
