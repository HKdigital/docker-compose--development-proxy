# Local proxy for frontend and backend development

## About

A common setup for production websites is to have a setup where:
- The frontend is a Single Page Application (SPA) that is served from the root of the domain.
- The backend is an API server that is served from a subpath on the same domain. E.g. /api.

During development it is handy to have the a setup that mimics the production setup: a frontend server and a backend server listening to requests on the same port. A (web) proxy can be used just for that purpose.

## Dependencies

This setup uses a `docker-compose` file, which means you must have [Docker Desktop](https://www.docker.com/products/docker-desktop) running on your machine.

## How it works

Clone this project using the terminal

```bash
git clone --depth 1 \
  https://github.com/HKdigital/docker-compose--development-proxy.git \
  development-proxy
```

Use a terminal and go to the cloned folder and start the proxy using `docker-compose`:

```bash
cd development-proxy
docker-compose up -d
```

Check if the container is running:

```bash
docker-compose ps
```

## What happens?

 This project uses a docker image `hkdigital/nginx`. By setting an environment property `INSTALL_DEFAULTS=development-proxy` in the docker-compose file, the image is instructed to install the configuration files for a local development proxy.

Traffic from the incoming port is then proxied to ports where the local development servers should listen.

## Ports

### Port for incoming requests

By default the proxy listens to port `9999` for incoming requests(see `docker-compose.yml`).

By default the proxy forwards the incoming requests to four locations:

### The backend API server

Request URL's that start with `/api` will be redirected to a (backend) server listening on the host on port `3333`.

### The backend WS server

Request URL's that start with `/ws` will be redirected to a (websocket) server listening on the host on port `5555`.

### The frontend HMR server

Request URL's that start with `/hmr` will be redirected to a (websocket) server listening on the host on port `8888`.

HMR stands for Hot Module Replacement and is used by e.g. the development server [Vite](https://vitejs.dev/).

### The frontend server

All other request URL's will be redirected to a (frontend) server listening on the host on port `8888`.

## Extend the setup with a public URL

To MIMIC the production setup a bit better or to share your development setup with others, you can add a public URL service like [NGROK](https://ngrok.com/) to the setup.

Setup a public URL using the NGROK tool and point it to the host port `9999` where the proxy is listening on.

# Buy me a coffee

If you like our work and would like us to share some more code, please support us:

https://www.buymeacoffee.com/hkdigital
