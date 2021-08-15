[![Build Status](https://img.shields.io/github/workflow/status/honestventures/docker-steamcmd-wine/Build%20Images.svg?logo=github)](https://github.com/honestventures/docker-steamcmd-wine/actions)
[![CodeFactor](https://www.codefactor.io/repository/github/honestventures/docker-steamcmd-wine/badge)](https://www.codefactor.io/repository/github/honestventures/docker-steamcmd-wine)
[![Steam Service Status](https://img.shields.io/static/v1?label=service&message=steam%20status&color=blue)](https://status.steamcmd.net)
[![Docker Pulls](https://img.shields.io/docker/pulls/honestventures/steamcmd-linux-wine.svg)](https://hub.docker.com/r/honestventures/steamcmd-linux-wine)
[![Image Size](https://img.shields.io/docker/image-size/honestventures/steamcmd-linux-wine/latest.svg)](https://hub.docker.com/r/honestventures/steamcmd-linux-wine)
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

# SteamCMD Docker Linux Images with Wine
SteamCMD on various Docker base images for downloading and running Steam games
and game server software. Dockerfiles for Linux base images include steamcmd, wine32 and wine64 installed.

For detailed information about SteamCMD,
see [wiki](https://developer.valvesoftware.com/wiki/SteamCMD).
Are you looking for a programmatic way to retrieve information via SteamCMD,
have a look at [steamcmd.net](https://www.steamcmd.net).

## Tags

*   [`ubuntu-20`, `ubuntu`, `latest`](dockerfiles/ubuntu-20/Dockerfile)
*   [`ubuntu-18`](dockerfiles/ubuntu-18/Dockerfile)
<!---
*   [`alpine-3`, `alpine`](dockerfiles/alpine-3/Dockerfile)
*   [`centos-8`, `centos`](dockerfiles/centos-8/Dockerfile)
*   [`centos-7`](dockerfiles/centos-7/Dockerfile)
--->

## Usage

*   [`docker.io`](https://hub.docker.com/repository/docker/honestventures/steamcmd-linux-wine)

### Pull latest image
```shell
docker pull honestventures/steamcmd-linux-wine:latest
```
### Test interactively
```shell
docker run --entrypoint /bin/sh -it honestventures/steamcmd-linux-wine:latest
```
### Download CSGO
```shell
docker run -it honestventures/steamcmd-linux-wine:latest +login anonymous +app_update 740 +quit
```
### Download CSGO to local mounted directory "data"
```shell
docker run -it -v $PWD:/data honestventures/steamcmd-linux-wine:latest +login anonymous +force_install_dir /data +app_update 740 +quit
```

## License

[MIT license](LICENSE)
