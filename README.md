# alpine-ssh

This is an automated build for [reverie89/alpine-ssh](https://hub.docker.com/r/reverie89/alpine-ssh/).

Checks daily for updates to the [LinuxServer Alpine Docker base image](https://github.com/linuxserver/docker-baseimage-alpine).

## Features
- OpenSSH
- rsync
- LinuxServer [Docker Mods](https://github.com/linuxserver/docker-mods)

## Usage example with Tailscale mod
docker-compose.yaml
```sh
services:
  alpine-ssh:
    image: reverie89/alpine-ssh
    container_name: alpine-ssh
    restart: always
    environment:
      - ROOT_PASSWORD=example-password
      - DOCKER_MODS=ghcr.io/tailscale-dev/docker-mod:main
      - TAILSCALE_STATE_DIR=/var/lib/tailscale
      - TAILSCALE_HOSTNAME=yourmachine-hostname-here
      - TAILSCALE_USE_SSH=1
      - TAILSCALE_AUTHKEY=xxxxxxx
    ports:
      - 2222:22
    volumes:
      - /path/to/tailscale-state:/var/lib/tailscale
      - /path/to/config:/etc/ssh
      - /path/to/authorized_keys:/root/.ssh
      - /path/to/container:/path/to/container
```

- How to use Tailscale mod: https://github.com/tailscale-dev/docker-mod
- You can remove the port mapping if you use [Tailscale SSH](https://tailscale.com/kb/1193/tailscale-ssh).
- Define `ROOT_PASSWORD` env if you are accessing with password.
- Volume mount `/etc/ssh` and `/root/.ssh` for data persistence. Edit sshd_config accordingly, then restart container to take effect.
- `docker exec {container_name} ssh-keygen -A` if you need to generate a key.
- If you need to access the shell: `docker exec -it {container_name} /bin/sh`.