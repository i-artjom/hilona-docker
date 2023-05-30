# HI-LONa installation with docker

## Running MediaWiki on local machine

First, start with the docker-compose.yml provided at DockerHub (https://hub.docker.com/_/mediawiki) by `running docker compose up` from the corresponding folder.

MediaWiki will then be accessible at http://localhost:8080.

Initially, the installation of MediaWiki needs to be performed through the browser using the default settings. The credentials specified in the YAML file (the host is set as database in the YAML, which should also be entered in the browser) need to be used.

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

Once the installation is complete, a `LocalSettings.php` file will be generated and can be downloaded. This file should be copied to the root folder along with the `docker-compose.yaml` file and then mapped as a volume in the compose file.

To do this, uncomment the last line:

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
      # After initial setup, download LocalSettings.php to the same directory as
      # this yaml and uncomment the following line and use compose to restart
      # the mediawiki service
      # ./LocalSettings.php:/var/www/html/LocalSettings.php
```

Now, restart by running `docker compose down` followed by `docker compose up`.

Settings (including the username and password for the database) can now be managed in the `LocalSettings.php` file.

