# dokku-livebook

Run Elixir Livebooks in Dokku

# Configuration

Configuration is set using direnv.

See `.envrc`.

To change values, create a file called `.envrc.private`
with the overrides.

* DOKKU_APP - the name of the Dokku app,
* DOKKU_HOST - the host where the Dokku runs,
* APP_DOMAIN - the hostname where the Livebook server will be available,
* DOMAIN_EMAIL - the email used when requesting the letsencrypt certificate
* LIVEBOOK_PASSWORD

# Setup

```sh
dokku apps:create $DOKKU_APP
dokku domains:set $DOKKU_APP $APP_DOMAIN
dokku config:set --no-restart $DOKKU_APP \
  LIVEBOOK_PASSWORD=$LIVEBOOK_PASSWORD \
  DOKKU_LETSENCRYPT_EMAIL=$DOMAIN_EMAIL
git remote add dokku dokku@$DOKKU_HOST:$DOKKU_APP
git push dokku
```

Get a TLS certificate

```sh
dokku config:set --no-restart $DOKKU_APP DOKKU_LETSENCRYPT_SERVER=staging
dokku letsencrypt:enable $DOKKU_APP
dokku config:unset --no-restart $DOKKU_APP DOKKU_LETSENCRYPT_SERVER
dokku letsencrypt:enable $DOKKU_APP
```

Save livebooks to a directory on the Docker host


```sh
export APP_DATA={{some path on the Dokku host}}
dokku docker-options:add $DOKKU_APP deploy "-v $APP_DATA:/data"
```
