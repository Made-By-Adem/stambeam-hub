# Stambeam Hub

[Stambeam](https://stambeam.nl) is genealogy software for your own computer (Windows and Mac). The **hub** is a small server you host yourself. Use it to share family trees with relatives or a genealogical society, and to connect the Stambeam phone app (iPhone and Android) to your family tree.

The hub is **optional**: Stambeam works fully without it. Your data stays on your own devices and on your own hub, and the hub makes no outgoing connections: no telemetry, no online licence check, no automatic updates.

- Website: <https://stambeam.nl/en/>
- What a hub is: <https://stambeam.nl/en/docs/what-is-a-hub/>
- Step-by-step installation: <https://stambeam.nl/en/docs/installing-the-hub/>

## The image

`ghcr.io/made-by-adem/stambeam-hub`, for `linux/amd64` and `linux/arm64` (a server, a NAS or a Raspberry Pi 4/5). Every version has its own tag (for example `:0.21.1`); the newest is also `:latest`. Run the same version as Stambeam on your computer.

```sh
docker pull ghcr.io/made-by-adem/stambeam-hub:latest
```

## Run with Docker Compose

```yaml
services:
  stambeam-hub:
    image: ghcr.io/made-by-adem/stambeam-hub:latest
    restart: unless-stopped
    ports:
      - '8080:8080'
    volumes:
      - stambeam-hub-data:/data
    environment:
      BACKUP_AT: '03:00' # daily backup, UTC

volumes:
  stambeam-hub-data:
```

```sh
docker compose up -d
docker logs stambeam-hub    # shows the one-time setup code for the first account
curl http://localhost:8080/v1/health
```

- **The `/data` volume** holds everything: accounts, family trees, photos and backups. Never delete it.
- **Port 8080** speaks plain HTTP. Put a reverse proxy with HTTPS in front of it if the hub must be reachable from outside your network.
- **An off-device backup is your responsibility**: the daily copy is stored on the same disk.

## Updating

```sh
docker compose pull
docker compose up -d
```

The volume stays. Update the hub to the same version as the app.

## Questions

Mail <info@stambeam.nl>. The source code of the hub is not public; this repository only holds this description.
