# Full stack for Symfony 5

[![Build Status](https://www.travis-ci.com/qnixnet/symfony_docker.svg?branch=master)](https://www.travis-ci.com/qnixnet/symfony_docker)

## Basic info

* [Nginx](https://nginx.org/)
* [PHP-FPM](https://php-fpm.org/)
* [Postgres](https://www.postgresql.org/)
* [Composer](https://getcomposer.org/)
* [Symfony CLI](https://symfony.com/)

## Previous requirements

This stack needs [docker](https://www.docker.com/) and [docker-compose](https://docs.docker.com/compose/) to be installed.

## Installing

1. Clone this repository

2. Modify configuration:
    - In the .env file
    - Add domains and email addresses to init-letsencrypt.sh

3. Run the init script:

    ```sh
    ./init-letsencrypt.sh
    ```

4. After installation run the server:

    ```sh
    docker-compose up
    ```

5. Next go inside in container:

    ```sh
    docker exec -it app bash
    ```

6. Install new Symfony project:

    ```sh
    symfony new symfony --dir=/var/www/symfony
    ```

7. Check the installation:

    ```sh
    symfony book:check-requirements
    ```

## How does it work?

We have the following *docker-compose* built images:

* `nginx`: The Nginx webserver container in which the application volume is mounted.
* `app`: The PHP-FPM container in which the application volume is mounted too.
* `postgres`: The Postgres database container.