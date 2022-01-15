[<-- Back to main section](../README.md)

# Running TYPO3

## Create TYPO3 project

For the first TYPO3 setup make sure [composer](https://getcomposer.org/) is installed.
Then run `make shell` or (`docker-compose exec --user application app /bin/bash`) to start an interactive shell in a container with docker-compose.
You will be located inside `app` folder.

If you started docker container, you may see PHP info after opening <http://localhost:8000>.

[^note]:
    If it shows error of PHP version, I'd recommend to use the last stable version (in my case it is 7.4). Then change Dockerfile.development file (line 52 "FROM webdevops/php-apache-dev:7.4"). After that, rebuild container ([Customizing](../CUSTOMIZE.md)).

Then follow the next steps:

```bash
rm -rf ./*
composer create-project typo3/cms-base-distribution /app
touch public/FIRST_INSTALL .gitkeep
```

Open <http://localhost:8000> and follow installation wizard.

When asked for database setup, use the settings from the [services documentation](https://github.com/webdevops/TYPO3-docker-boilerplate/blob/master/documentation/SERVICES.md#mysql) or see your `etc/environment.yml`. Make sure to set the correct database-host ('mysql' by default).

[^note]:
    In step 2, by default used connection 1 (\[MySQLi\] Manually configured MySQL TCP/IP connection). Then in `Host` filed type `mysql`. The port number is the same as in `docker-compose.yml`.
    Use credentials from `etc/environment.yml`.

Feel free to modify your TYPO3 installation in your `app/` (a shared folder of Docker),
most of the time there is no need to enter any Docker container.


## TYPO3 cli runner

You can run one-shot command inside the `app` service container:

```bash
docker-compose run --rm app web/typo3/cli_dispatch.phpsh extbase
# or simply:
docker-compose run --rm app cli extbase   # cli refers to CLI_SCRIPT env in etc/environment.yml

docker-compose run --rm app bash
```


## Error: Trusted Host pattern

Set in web/typo3conf/LocalConfiguration.php:

    'SYS' => array(
        [ ... ],
        'trustedHostsPattern' => '.*',
    ),

Should not be needed on Apache HTTPd.
