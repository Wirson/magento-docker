config based on https://dockerfile.readthedocs.io/en/latest/content/DockerImages/dockerfiles/php-nginx-dev.html

Create folder structure as:
```bash
--magento
  --docker - place this docker configuration here
  |
  --magento - your magento codebase root
```

# First steps
1. create project root ex. `mkdir ~/Sites/magento; cd ~/Sites/magento;`
3. clone docker setup into `docker` directory `git@github.com:Wirson/magento-docker.git docker`
4. copy `env.php` - `cp docker/env.php.local magento/app/etc/env.php`
5. create containers `bin/start` or `docker compose up -d`
6. add local dns entry  `sudo -- sh -c "echo '127.0.0.1 local.magento.com' >> /etc/hosts"`
7. import DB `bin/db_import db.sql` or `docker exec -i mysql mysql -uroot -proot magento < ./db.sql`
8. enter php container `docker exec -it web bash`
9. `cd app`
10. install composer dependencies `composer install`
11. install magento `php bin/magento setup:upgrade`
12. run reindex `php bin/magento indexer:reindex`

# Usage
1. `cd ~/Sites/magento/docker` and use `bin/start` to start all the containers
2. to flush all the caches, use `bin/cache_flush` - use space separated, optional arguments ex. `bin/cache_flush block_html fullpage`
3. to upgrade app, ex. after new module creation, use `bin/setup_upgrade`
4. `bin/db_import {{path_to_ .sql}}` will drop and import new db dump - remember to secure your data

* see `bin` dir for available, most frequently used commands - feel free to add your own!
* to enter `mysql` container run - `docker exec -it mysql bash`
* to enter `php nginx` container run - `docker exec -it web bash`

You can encounter ElasticSearch related errors in ubuntu. In this case, run `sudo sysctl -w vm.max_map_count=262144` in your host machine.

use following data to configure DB client (ex. `dBeaver`)  
host `127.0.0.1`  
port `3306`  
user `root`  
pass `root`

### Todo
* xDebug does not work for CLI php commands - now it should, but consider changing `XDEBUG_CLIENT_HOST` in `compose.yml`
* implement mailcatcher/mailhog