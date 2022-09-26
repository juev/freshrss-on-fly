# freshrss-on-fly


## Prerequisites

 - a [fly.io](https://fly.io/) account
 - a clone of this repository

All commands should be run in the directory of your local clone of this repository.

## Install flyctl

Follow [the instructions](https://fly.io/docs/getting-started/installing-flyctl/) to install fly's command-line interface `flyctl`.

Then, [log into flyctl](https://fly.io/docs/getting-started/log-in-to-fly/).

```sh
flyctl auth login
```

## Create app

```sh
# tell fly.io you wish to create a new application in the amsterdam region (there are many other regions you could pick too)
# pick any name for the app that you'd like, in the example we are using `fresh-on-fly`
flyctl launch --name fresh-on-fly --no-deploy --region ams
```

## Add a persistent volume

Create a [persistent volume](https://fly.io/docs/reference/volumes/). Fly's free tier includes `3GB` of storage across your VMs. Since `fresh` is very light on storage, a `1GB` volume will be more than enough for most use cases. It's possible to change volume size later. A how-to can be found in the _"scale persistent volume"_ section below.

```sh
# give the newely create application persistant storage, so your data persists between app updates
flyctl volumes create fresh_data --size 1 --region ams --app fresh-on-fly
```

## Configuration

Change `fly.roml` file, set domain name (custom or fly.io):

```toml
  SERVER_DNS="fresh.example.com"
```

## Deploy to fly

Deploy the application to fly.

```sh
flyctl deploy
```

## Optional: Create certificate for your domain

If you do not use Cloudflare proxy, and you use custom domain, you need to issue
certificates ([Custom Domains and SSL Certificates](https://fly.io/docs/app-guides/custom-domains-with-fly/#teaching-your-app-about-custom-domains)):

```sh
flyctl certs create example.com
```

Thats all!
