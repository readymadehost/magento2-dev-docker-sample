# Magento2 dev docker sample - ReadyMadeHost

A magento2 sample project using magento2 dev docker


## Project setup

- `git clone https://github.com/readymadehost/magento2-dev-docker.git magento2-sample-docker`
- `cd magento2-sample-docker`
- `git clone https://github.com/readymadehost/magento2-dev-docker-sample.git project`
- `cp .env.sample .env` and review `.env` file
- `docker-compose build`
- `docker-compose up -d`
- `docker-compose exec cli bash`
- `composer install` to install php packages
- `mpp` to manage project permission
- Run `bin/magento` and should return list of commands
- Magento2.4.x project install command https://devdocs.magento.com/guides/v2.4/install-gde/install/cli/install-cli.html
- Setup sample data `bin/magento sampledata:deploy`
- `bin/magento setup:upgrade`
- Run bash alis `mpp` for `/root/manage-project-permission.sh`

#### Install for Magento2.4.x

```
bin/magento setup:install \
--base-url=http://localhost:8080 \
--db-host=mariadb \
--db-name=project \
--db-user=root \
--db-password=root \
--backend-frontname=admin \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin@123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=elasticsearch \
--elasticsearch-port=9200
```


## Setting magento2 composer auth with github actions

- Goto: Repository > Settings > Secrets > New repository secret
- Name: `COMPOSER_AUTH_JSON` and Value as needed

```json
{
    "http-basic": {
        "repo.magento.com": {
            "username": "<public-key>",
            "password": "<private-key>"
        }
    }
}
```


## Magento2 dev docker

- https://github.com/readymadehost/magento2-dev-docker


## Running tests in dev-docker

- `docker-compose exec -T mariadb mysql -u root -proot -e "DROP DATABASE magento_integration_tests;"` to drop magento_integration_tests database if exist
- `docker-compose exec -T mariadb mysql -u root -proot -e "CREATE DATABASE magento_integration_tests;"` to create fresh test database
- `docker-compose exec cli bash`
- `cp magento2-dev-docker/install-config-mysql.php dev/tests/integration/etc/install-config-mysql.php`
- `rm -rf generated/* && bin/magento cache:clean`
- `bin/magento dev:tests:run` to run tests


## Running tests in github actions

Magento2 dev docker has database, extensions, elasticsearch and everything needed to work with magento2 tests. Github actions are configured for sample project, check https://github.com/readymadehost/magento2-dev-docker-sample/blob/master/.github/workflows/tests.yml

Create an issue if you have any questions.
