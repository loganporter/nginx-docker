# Boilerplate for nginx with Let’s Encrypt on docker-compose

> This repository is based on a [step-by-step guide on how to
> set up nginx and Let’s Encrypt with Docker](https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71).

BASED ON https://github.com/wmnnd/nginx-certbot.git

`init-letsencrypt.sh` fetches and ensures the renewal of a Let’s
Encrypt certificate for one or multiple domains in a docker-compose
setup with nginx.
This is useful when you need to set up nginx as a reverse proxy for an
application.

Before running the script the NGINX conf need to be changed to host the temp keys.
Change all the `*.conf` files to the following format

```
server {
    listen 80;
    server_name <FULL DOMAIN HERE>;
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}
```

replacing `<FULL DOMAIN HERE>` to `loganporter.net` or `sso.loganporter.net` etc.

Then use the following steps
```bash
# Remove the current containers
dcs down

# Delete the old certs
cd data
sudo rm -rf ./certbot

# setup the new certs
cd ..
./init-letsencrypt.sh
```

The revert the `.conf` files to their original config and restart the containers with `dcs stop` and `dcs up -d`.

## Installation

1. [Install docker-compose](https://docs.docker.com/compose/install/#install-compose).

2. Clone this repository: `git clone https://github.com/wmnnd/nginx-certbot.git .`

3. Modify configuration:

- Add domains and email addresses to init-letsencrypt.sh
- Replace all occurrences of example.org with primary domain (the first one you added to init-letsencrypt.sh) in data/nginx/app.conf

4.  Run the init script:

        ./init-letsencrypt.sh

5.  Run the server:

        docker-compose up

## Got questions?

Feel free to post questions in the comment section of the [accompanying guide](https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71)

## License

All code in this repository is licensed under the terms of the `MIT License`. For further information please refer to the `LICENSE` file.
