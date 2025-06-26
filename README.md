# Traefik, DuckDNS, and Let's Encrypt Integration

This project demonstrates the integration of the following tools to create a secure, dynamic, and scalable reverse proxy solution:

- Traefik: a modern HTTP reverse proxy and load balancer
- DuckDNS: a free dynamic DNS service
- Let's Encrypt: a free, automated, and open certificate authority

## Requirements

- A server or a virtual machine with Docker and Docker Compose installed
- A DuckDNS account and domain

## Installation

1. Clone this repository to your server:

```
git clone https://github.com/scafer/traefik-duckdns-letsencrypt
```

2. Rename the `example.env` file to `.env` and set the environment variables with your values:

```
cd traefik-duckdns-letsencrypt
mv example.env .env
```

3. Replace `your-duckdns-token` with your DuckDNS token, `your-email` with your email address, and `your-duckdns-domain` with your duckdns domain (ex: your-domain.duckdns.org).

4. Start the containers with Docker Compose:

```
docker-compose up -d
```

5. Access whoami at `https://whoami.your-domain.duckdns.org` and make sure everything is working properly.

## Usage

To add a new service behind Traefik, simply create a new entry in the `services` section, following the example of the `whoami` service.

## Conclusion

With this setup, you can easily add and manage multiple services behind a single domain, with automatic HTTPS certificates from Let's Encrypt and dynamic DNS updates from DuckDNS.

Feel free to fork and customize this repository to fit your needs!
