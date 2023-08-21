# Budibase on Dokku

Budibase relies on a number of supporting services, but recently they rolled
up a single Docker image that handles all of it in a single place.

## Create the app and storage

```
dokku apps:create budibase
dokku storage:ensure-directory /var/lib/dokku/data/storage/budibase
dokku storage:mount budibase /var/lib/dokku/data/storage/budibase:/data
```

## Pushing the app

On your local machine, clone this repository, add your dokku remote, and push to dokku.

```
git clone <foo bar>
git add remote dokku dokku@your-dokku-server.com:arango
git push dokku
```

## Domain, proxy, and SSL

Setting up a domain may not be necessary if it will live at a subdomain of your global
dokku domain. Set up port mappings so nginx can route things properly; Arango handles
both encrypted and unencrypted traffic on port 8529, so we just point everything there.

```
dokku domains:set budibase budibase.your-dokku-server.com

dokku ports:add budibase http:80:80
dokku ports:add budibase https:443:443

dokku letsencrypt:enable budibase
```