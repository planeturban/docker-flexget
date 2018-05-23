# planeturban/docker-flexget

Read all about FlexGet [here](http://www.flexget.com/#Description) 

This container launches multiple instances of flexget, just drop your config files in your config directory; all includes (such as secrets) should be placed in a directory since one can't run those. 

## Usage

```
docker create \
    --name=flexget \
    -e PGID=<gid> -e PUID=<uid> \
    -e FLEXGET_LOG_LEVEL=debug \
    -e PIP_PACKAGES="extra packages to install"
    -e UPDATE_FLEXGET="true"
    -e UPDATE_PACKAGES="true"
    -v <path to data>:/config \
    -v <path to downloads>:/downloads \
    planeturban/docker-flexget
```

This container is based on phusion-baseimage with ssh removed. For shell access whilst the container is running do `docker exec -it flexget /bin/bash`.

**Parameters**

* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e FLEXGET_LOG_LEVEL` for logging level - see below for explanation
* `-e PIP_PACKAGES` additional packages such as transmissionrpc
* `-e UPDATE_FLEXGET` perform Flexget update on start.
* `-e UPDATE_PACKAGES` perform pip packages update on start.
* `-v /config` - Location of FlexGet config.yml (DB files will be created on startup and also live in this directory)
* `-v /downloads` - location of downloads on disk

**Daemon mode**

This container runs flexget in [daemon mode](https://flexget.com/Daemon). This means by default it will run your configured tasks every hour after you've started it for the first time. If you want to run your tasks on the hour or at a different time, look at the [scheduler](https://flexget.com/Plugins/Daemon/scheduler) plugin for configuration options. Configuration is automatically reloaded every time just before starting the tasks as scheduled, to apply your changes immediately you will need to restart the container.

**Logging Level**

Set the verbosity of the logger. Optional, defaults to debug if not set. Levels: critical, error, warning, info, verbose, debug, trace.

### User / Group Identifiers

**TL;DR** - The `PGID` and `PUID` values set the user / group you'd like your container to 'run as' to the host OS. This can be a user you've created or even root (not recommended).

Part of what makes this container work so well is by allowing you to specify your own `PUID` and `PGID`. This avoids nasty permissions errors with relation to data volumes (`-v` flags). When an application is installed on the host OS it is normally added to the common group called users, Docker apps due to the nature of the technology can't be added to this group. So this feature was added to let you easily choose when running your containers.

[Flexget CLI](https://flexget.com/CLI)

## Updates / Monitoring

* Upgrade to the latest version of FlexGet simply `docker restart flexget` with environment varible UPDATE_FLEXGET set to anything.
* Monitor the logs of the container in realtime `docker logs -f flexget`.

**Credits**
* [linuxserver.io](https://github.com/linuxserver) for providing awesome docker containers.
* [cpoppema](https://github.com/cpoppema/docker-flexget) cpoppema/docker-flexget
