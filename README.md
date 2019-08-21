# AutoSSL nginx server

A web server that serves HTML with automatic SSL courtesy of certbot

## Config

You'll need to put your server name in docker/data/nginx/app.conf for the SSL cert to be issued properly. Replace $SERVER_NAME in the nginx config below with your domain.

It is NOT a clever idea to remove this from data/nginx/app.conf:

```
server {
    listen 80;
    server_name $SERVER_NAME;
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}
```

If you remove this, odd things will start happening and certificates will no longer renew. If you're using this repo as a template for other things (as I was...) then make sure you keep this config block in when you're extending things!

## Dev

- Also included are files for building a complete Docker container with the static HTML built in.
- There's also a very simple Python server, `server.py` for simple local dev. There are MANY other options for this sort of thing, but there we are 