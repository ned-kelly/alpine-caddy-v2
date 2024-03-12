# Caddy v2 Server on Alpine Linux

[![](https://badge.imagelayers.io/bushrangers/alpine-caddy-v2:latest.svg)](https://imagelayers.io/?images=bushrangers/alpine-caddy-v2:latest 'Get your own badge on imagelayers.io') [![Docker Pulls](https://img.shields.io/docker/pulls/bushrangers/alpine-caddy-v2.svg?maxAge=2592000)](https://hub.docker.com/r/bushrangers/alpine-caddy-v2/)
![buildx](https://github.com/ned-kelly/alpine-caddy/workflows/buildx/badge.svg)

This is a [Docker](https://www.docker.com/) image for [Caddyserver](https://caddyserver.com/). (Note if you are looking for Caddy v1, please refer to ned-kelly/alpine-caddy) This image runs with a base of [Alpine-Linux](http://www.alpinelinux.org/) making it extremely small, secure and fast.

**This image can be found on [Docker Hub](https://hub.docker.com/r/bushrangers/alpine-caddy-v2/) - and is automatically built each week using the latest official (stable) release from caddyserver.com.**

## Usage
We recommend using our images in conjunction with [Docker-Compose](https://docs.docker.com/compose/). This allows for easier creation of containers with the proper volumes and ports enabled.

We have included an [example docker-compose](https://github.com/ned-kelly/alpine-caddy/tree/master/examples/docker-compose.example.yml) file for use in a real project.

This image works with two defaults

1. A default [Caddyfile](https://github.com/ned-kelly/alpine-caddy-v2/tree/master/Caddyfile)
2. A default location inside the container for static files: /var/www/html

In order to use this image, we recommend running it with a volume connecting your static files to the root location of the docker file:

    docker run -d -p 80:80 -v $(pwd)/public:/var/www/html bushrangers/alpine-caddy-v2

The server will be available at your.docker.machine.ip.

This is the bare minimum needed to use this image. Although further customization is made easier with a docker-compose file.

The benefits of building an image with a overrideable Caddyfile are that you can   include your own by including another volume. To see a fully configured docker-compose file see this [example](https://github.com/ned-kelly/alpine-caddy-v2/tree/master/examples/docker-compose.example.yml).

For writing a custom Caddyfile please read [this](https://caddyserver.com/docs/caddyfile).

### Middleware

Alpine-Caddy-v2 includes all Caddy Middleware and features. You can read more on these specific features in the [Caddy User Guide](https://caddyserver.com/docs).

### Caddy as a reverse proxy

This image can also effectively be used as a reverse proxy. Included in the examples/ folder is an [example Caddyfile](https://github.com/ned-kelly/alpine-caddy-v2/tree/master/examples/Caddyfile.proxy.example).

The [example docker-compose](https://github.com/ned-kelly/alpine-caddy-v2/tree/master/examples/docker-compose.proxy-example.yml) shows how to include your custom Caddyfile as a volume as well as an example proxy set up with containers.

## Volumes

Alpine-Caddy-v2 has three locations where volumes can be linked to.

### Static Files

In order to serve static content, alpine-caddy-v2 needs to be able to access your static files from inside of the container. To do this, link the directory of your static files with /var/www/html inside of the container.

For docker-compose.yml files, under the volumes declaration, include:

    -  ./public:/var/www/html

or

    docker run -v $(pwd)/public:/var/www/html

### Custom Caddyfile

To upload a custom Caddyfile, link your Caddyfile to the directory /etc/Caddyfile in the container.
For docker-compose.yml files, under the volumes declaration, include:

    -  ./Caddyfile:/etc/Caddyfile

or

    docker run -v $(pwd)/Caddyfile:/etc/Caddyfile bushrangers/alpine-caddy-v2

### Certificate Persistance

If you use alpine-caddy-v2 to generate SSL certificates from [Let's Encrypt](https://letsencrypt.org/), you should persist those certificates outside of the container. In the instance of a container failure, this allows the container to reuse the same certificates, instead of generating new ones from Let's Encrypt.

For information on including this into your Caddyfile see the [Caddyfile tls specification](https://caddyserver.com/docs/tls).

The certificates are stored in /root/.caddy inside of the container, and thus you must connect an outside directory to that directory to allow persistance. For docker-compose.yml files, under the volumes declaration, include:

    -  ./.caddy:/root/.caddy

or

    docker run -v $(pwd)/.caddy:/root/.caddy

## Plugins Enabled in this Build

By default the following additional plugins have been included with this build _(you can read about them on their respective github web pages)_:

```
 
 ## SERVER TYPES - Things Caddy can serve

- net
- dns
- supervisor

## DNS PROVIDERS - Obtain certificates using DNS

- dns.providers.azure
- dns.providers.cloudflare
- dns.providers.digitalocean
- dns.providers.googleclouddns
- dns.providers.linode
- dns.providers.route53

## EVENT HOOKS - Plugins that are triggered by events

- hook.service

## CADDYFILE LOADERS - Ways to load the Caddyfile
- Caddyfile
- docker

## DIRECTIVES/MIDDLEWARE - Extra functionality with the Caddyfile

- http.handlers.lambda
- http.handlers.cache
- http.handlers.filter
- http.handlers.rate_limit
- http.handlers.realip
 
```

## License

The code is available under the [MIT License](https://github.com/ned-kelly/alpine-caddy-v2/tree/master/LICENSE).
