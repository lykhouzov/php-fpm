# A Set of php-fpm containers.

These containers mostly build for Magento 1.x development.
But could be used for aother type of php development

## Actual versions
- php 5.4
- php 5.5
- php 5.6
- php 7.0

container with php7.0 doesn't include ioucube.

## Example of docker-compose usage
```
php:
  image: lykhouzov/php55
  privileged: true
  mem_limit: 2g
  environment:
    - MAGE_ROOT=/var/www/html/magento
    - MAGE_CRON_EXPR=*/2 * * * *
  volumes:
    - ./:/var/www/html/magento/
  links:
    - redis
    - db
    - solr
```
