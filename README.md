# docker-env-for-drupal9
docker environment for drupal

## Docker Environment stack

- php:8.0-apache
- composer
- mysql
- phpmyadmin
- redis

## Environment Install

at the same level as the docker-compose.yml file (here root !) : `docker compose build` and then `docker compose up -d`.
Here the work directory is `/var/www`.

## vhosts.conf

vhosts.conf is already configured for drupal.
This file links to `/var/www/drupal/web`. You have to modify `drupal` by the name of your directory name project.

## Drupal Install

at the root of the repo : `composer create-project drupal-composer/drupal-project:9.x-dev some-dir --no-interaction`. You have to modify `some-dir` by your directory name's project.

## Drush Install

Drush allows, in particular, to enable the installed modules, in command lines.
In root of your drupal directory, at the same level of `composer.json` file : `composer require predis/predis` `composer require drush/drush`  `cp modules/contrib/redis/example.services.yml sites/default/redis.services.yml`

## Redis

### Install in Drupal
In root of your drupal directory, at the same level of `composer.json` file : `composer require 'drupal/redis:^1.6'`.
Then you have to enable Redis, in drupal extensions : in `var/www/directory-drupal-project/vendor/bin` => `./drush en redis` and then `./drush cr` to clear cache.

### Configure settings.php
This file is located in `directory-drupal-project/web/sites/default/settings.php`

Put these lines at the end of this file :
$settings['redis.connection']['interface'] = 'Predis';
$settings['redis.connection']['host'] = 'redis';
$settings['redis.connection']['port'] = '6379';
$settings['cache']['default'] = 'cache.backend.redis';
$settings['container_yamls'][] = 'sites/default/redis.services.yml';

verify in localhost:8741/admin/reports/redis url.

CONGRATULATIONS, YOUR DRUPAL PROJECT WITH REDIS WORKS !!!