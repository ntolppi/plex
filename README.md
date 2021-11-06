# Home Plex Server
Plex server using Traefik for reverse proxy and acme Let's Encrypt SSL certificate handling.

## About
Uses Traefik to route requests to plex.nzti.net to plex backend container
Routes are configured in the [docker-compose.yml labels](https://github.com/SubZero12556/plex/blob/e6112baf98836bc34048f7131cbe22e47508fbd7/docker-compose.yml#L31)
```
labels:
  - traefik.http.routers.plex.rule=Host(`plex.nzti.net`)
```

## Setup
### Add Plex Claim Token
1. Go to plex.tv/claim to generate the claim token
2. Add claim token to [docker-compose.yml PLEX_CLAIM](https://github.com/SubZero12556/plex/blob/e6112baf98836bc34048f7131cbe22e47508fbd7/docker-compose.yml#L28) environment variable for plex service

### Add email for Let's Encrypt acme
1. Add email to [traefik.toml](https://github.com/SubZero12556/plex/blob/e6112baf98836bc34048f7131cbe22e47508fbd7/traefik.toml#L22)

### Start Traefik and Plex
```bash
docker-compose up -d
```

### Setup Plex
1. Navigate to https://plex.nzti.net/setup
2. Setup Plex with name and mapping to volumes
3. Ready to use Plex with a certificate setup!

## Troubleshooting
### Acme.json permission / file not found error
The docker-compose volumes has letencrypt, which should auto be created when running up, if it isn't create a letsencrypt directory
```bash
mkdir letsencrypt
```
In the letsencrypt directory an acme.json file will be created, DO NOT MAKE ONE YOURSELF
Making acme.json in letsencrypt can cause Let's Encrypt certificate generation to fail silently giving the error:
```
Resolver not found for le
```

### Challenge response required for ACME certificate generation
Error in acme certificate generation caused by the httpChallenge endpoint from being reachable.
Make sure port 80 is reachable.

## External Documentation
* [Plex](https://support.plex.tv/articles/200264746-quick-start-step-by-step-guides/)
  * [Plex Docker](https://hub.docker.com/r/linuxserver/plex)
* [Traefik](https://doc.traefik.io/traefik/)
  * [Traefik Docker](https://hub.docker.com/_/traefik)
  * [Let's Encrypt ACME](https://doc.traefik.io/traefik/https/acme/)
