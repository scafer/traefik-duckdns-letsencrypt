# Traefik, DuckDNS, and Let's Encrypt Integration

This project demonstrates the integration of the following tools to create a secure, dynamic, and scalable reverse proxy solution:

- Traefik: a modern HTTP reverse proxy and load balancer
- DuckDNS: a free dynamic DNS service
- Let's Encrypt: a free, automated, and open certificate authority

## Requirements

- A server or a virtual machine with Docker and Docker Compose installed
- A DuckDNS account and domain

## Environment Variables

| Variable | Description | Example |
|---|---|---|
| `ACME_PROVIDER` | DNS challenge provider | `duckdns` |
| `ACME_EMAIL` | Email for Let's Encrypt notifications | `you@example.com` |
| `DOMAIN` | Your full DuckDNS domain | `your-domain.duckdns.org` |
| `DUCKDNS_TOKEN` | Your DuckDNS API token | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

## Installation

1. Clone this repository to your server:

```
git clone https://github.com/scafer/traefik-duckdns-letsencrypt
```

2. Rename the `example.env` file to `.env` and fill in your values:

```
cd traefik-duckdns-letsencrypt
mv example.env .env
```

3. Create the Let's Encrypt storage file with the correct permissions:

```
mkdir -p letsencrypt && touch letsencrypt/acme.json && chmod 600 letsencrypt/acme.json
```

4. Start the containers with Docker Compose:

```
docker compose up -d
```

5. Access whoami at `https://whoami.your-domain.duckdns.org` and make sure everything is working properly.

## Usage

### Traefik Dashboard

The Traefik dashboard is available at `https://dashboard.your-domain.duckdns.org`.

> **Security warning:** The dashboard is publicly accessible without authentication by default. Before exposing this setup to the internet, add a `basicAuth` middleware to the dashboard router.

### Adding a New Service

To add a new service behind Traefik, add an entry to the `services` section in `docker-compose.yml` with the required labels:

```yaml
services:
  my-service:
    image: my-image
    restart: always
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.my-service.rule=Host(`my-service.your-domain.duckdns.org`)"
      - "traefik.http.routers.my-service.entrypoints=websecure"
```

If the container exposes more than one port, also specify the port Traefik should forward to:

```yaml
      - "traefik.http.services.my-service.loadbalancer.server.port=8080"
```

## Troubleshooting

**Certificate not being issued**

Check the Traefik logs for ACME errors:

```
docker compose logs traefik
```

Common causes: incorrect `DUCKDNS_TOKEN`, invalid `ACME_EMAIL`, or Let's Encrypt rate limits.

**Permission error on `acme.json`**

Traefik requires strict permissions on the certificate storage file:

```
chmod 600 letsencrypt/acme.json
```

**Service not reachable**

Confirm Traefik can see the container and that the router is active in the dashboard under **HTTP > Routers**.

## Conclusion

With this setup, you can easily add and manage multiple services behind a single domain, with automatic HTTPS certificates from Let's Encrypt and dynamic DNS updates from DuckDNS.

Feel free to fork and customize this repository to fit your needs!
