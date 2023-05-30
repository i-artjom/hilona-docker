# HI-LONa Installation with Docker

## Starting the MediaWiki Container on the Local Machine if LocalSettings.php Already Exists

All the important settings for the Wiki, such as database credentials, appearance, user rights, and extensions, can be managed inside the 'LocalSettings.php' file. If this file already exists, it can be linked as a volume in the following way (last line `- ./LocalSettings.php:/var/www/html/LocalSettings.php`):

```yaml
services:
  mediawiki:
    image: mediawiki
    restart: always
    ports:
      - 8080:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      # After the initial setup, download LocalSettings.php to the same directory as
      # this YAML file, uncomment the following line, and use compose to restart
      # the mediawiki service
      - ./LocalSettings.php:/var/www/html/LocalSettings.php
```

The root folder should now contain the `LocalSettings.php` and `docker-compose.yml` files. To start the container, navigate to the root directory and run `docker compose up`. The Wiki should be accessible through a web browser via http://localhost:8080.

## Setting up MediaWiki if No LocalSettings.php File is Present

If there is no existing `LocalSettings.php` file, follow these steps:

1. Start by using the provided `docker-compose.yml` file from DockerHub (https://hub.docker.com/_/mediawiki) and run `docker compose up` from the corresponding folder.
2. MediaWiki will be accessible at http://localhost:8080.
3. Initially, the installation of MediaWiki needs to be performed through the browser using the default settings. Use the credentials specified in the YAML file (the database host specified in the YAML should also be entered in the browser).
```yaml
  database:
    image: mariadb
    restart: always
    environment:
      # @see https://phabricator.wikimedia.org/source/mediawiki/browse/master/includes/DefaultSettings.php
      MYSQL_DATABASE: my_wiki
      MYSQL_USER: wikiuser
      MYSQL_PASSWORD: example
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
    volumes:
      - db:/var/lib/mysql
```
4. Once the installation is complete, a `LocalSettings.php` file will be generated. Download this file and copy it to the root folder along with the `docker-compose.yaml` file.
5. Uncomment the line `- ./LocalSettings.php:/var/www/html/LocalSettings.php` in the `docker-compose.yml` file:
```yaml
services:
  mediawiki:
    image: mediawiki
    restart: always
    ports:
      - 8080:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      # After the initial setup, download LocalSettings.php to the same directory as
      # this YAML file, uncomment the following line, and use compose to restart
      # the mediawiki service
      # - ./LocalSettings.php:/var/www/html/LocalSettings.php
```
6. Restart by running `docker compose down`, followed by `docker compose up`.
7. The settings, including the database username and password, can now be managed in the `LocalSettings.php` file.
