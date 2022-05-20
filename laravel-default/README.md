# Docker

This directory contains the docker files for Entelechy's API. If you're not using docker, don't worry about it. Otherwise,
read on to find out how to setup docker for Entelechy.

It is copied in from the [Hex: Docker for Laravel template](https://github.com/hex-digital/docker-laravel-template).
Check this repo for updates if required.

## Prerequisites

- Install [Docker Desktop](https://docs.docker.com/docker-for-mac/install/) and ensure it's at least version 20.10.x.
- Ensure the main Entelechy repo is cloned down to your machine.
- (Optionally but recommended) Copy the `.env.dist` file in `/docker` and call it `.env`, then set your suffix and ports.

## Caveats

All Docker commands must be run from within this `/docker` directory. If not, the `.env` file for docker compose will
not load, and your container names and ports will not be correctly specified.

If you wish to run docker commands from outside the `/docker` directory, you must specify which `.env` file to load using
the `--env-file` argument, e.g. `docker-compose --env-file ./docker/.env up`.

## Usage

```
docker compose up -d site    # Spin up the services required to serve the API
docker compose down          # And spin them down again
```

### Usage Notes

Specifying `site` at the end of `docker compose up -d` will bring up only the container's required for the site. 
That means the single-use composer, artisan and npm containers won't start needlessly. However it's not required.

When the `site` container is brought up, the following containers will be spun up. The default port, if not changed
in `.env`, is displayed next to it.

- **nginx** - `:80`
- **mysql** - `:3306`
- **php** - `:9000`
- **redis** - `:6379`
- **mailhog** - `:8025`

`composer`, `npm` and `artisan` can also be run, without having to have them installed on your local machine. Examples
of running them are displayed below:

- `docker compose run --rm composer update`
- `docker compose run --rm npm run dev`
- `docker compose run --rm artisan migrate`

### Aliases

We recommend the below aliases for docker. [More useful ones can be found here](https://gist.githubusercontent.com/jgrodziski/9ed4a17709baad10dbcd4530b60dfcbb/raw/d84ef1741c59e7ab07fb055a70df1830584c6).

```
function dcr-fn {
  docker compose run $@
}

alias dcu="docker compose up -d"
alias dcd="docker compose down"
alias dcr=dcr-fn
```

This allows the shortening of commands such as `dcu site` or `dcr --rm composer update`.

## MailHog

MailHog is used to automatically trap outgoing mail so that it isn't sent. This is great for local development and testing emails.

The MailHog service is spun up along with the website containers. It has a UI in which all emails can be viewed. 

To view this dashboard and see any emails coming through the system visit [localhost:8025](http://localhost:8025) 
after running `docker-compose up -d site`. The port can be changed via `~/docker/.env`.
