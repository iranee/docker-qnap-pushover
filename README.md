# `qnap-pushover` &#x1F514;

[![Python](https://img.shields.io/github/languages/top/TheCatLady/docker-qnap-pushover?logo=python&style=flat-square)](https://github.com/TheCatLady/docker-qnap-pushover)
[![Docker Image Size](https://img.shields.io/docker/image-size/thecatlady/qnap-pushover/latest?style=flat-square&logo=docker&label=docker%20image%20size)](https://hub.docker.com/r/thecatlady/qnap-pushover)
[![Docker Hub Pulls](https://img.shields.io/docker/pulls/thecatlady/qnap-pushover?style=flat-square&label=docker%20hub%20pulls&logo=docker)](https://hub.docker.com/r/thecatlady/qnap-pushover)
[![GitHub Repo Stars](https://img.shields.io/github/stars/TheCatLady/docker-qnap-pushover?label=github%20stars&logo=github&style=flat-square)](https://github.com/TheCatLady/docker-qnap-pushover/stargazers)<br/>
[![Last Commit](https://img.shields.io/github/last-commit/TheCatLady/docker-qnap-pushover?style=flat-square&logo=git)](https://github.com/TheCatLady/docker-qnap-pushover)
[![GitHub Container Registry Build Status](https://img.shields.io/github/workflow/status/TheCatLady/docker-qnap-pushover/Docker?label=github%20container%20registry%20build&logo=github&style=flat-square)](https://github.com/TheCatLady/docker-qnap-pushover)
[![Docker Hub Build Status](https://img.shields.io/docker/cloud/build/thecatlady/qnap-pushover?style=flat-square&label=docker%20hub%20build&logo=docker)](https://hub.docker.com/r/thecatlady/qnap-pushover)<br/>
[![License](https://img.shields.io/github/license/TheCatLady/docker-qnap-pushover?logo=open%20source%20initiative&style=flat-square)](https://github.com/TheCatLady/docker-qnap-pushover/blob/main/LICENSE)
[![Become a GitHub Sponsor](https://img.shields.io/badge/github%20sponsors-become%20a%20sponsor-ff69b4?style=flat-square&logo=github%20sponsors)](https://github.com/sponsors/TheCatLady)
[![Donate via PayPal](https://img.shields.io/badge/paypal-make%20a%20donation-blue?style=flat-square&logo=paypal)](http://paypal.me/DHoung)

[Pushover](https://pushover.net/) notifications for [QNAP](https://www.qnap.com/) NAS system events via `python-pushover`

## Usage

Docker images are available from both [GitHub Container Registry (GHCR)](https://github.com/users/TheCatLady/packages/container/package/qnap-pushover) and [Docker Hub](https://hub.docker.com/r/thecatlady/qnap-pushover).

If you would prefer to pull from GHCR, simply replace `thecatlady/qnap-pushover` with `ghcr.io/thecatlady/qnap-pushover` in the examples below.

### Docker Compose (recommended)

```yaml
---
version: "2.1"
volumes:
  qnap-pushover:
services:
  qnap-pushover:
    image: thecatlady/qnap-pushover
    container_name: qnap-pushover
    environment:
      - TZ=America/New_York #optional
      - LOG_LEVEL=WARN #optional
      - NOTIFY_LEVEL=WARN #optional
      - POLL_INTERVAL=10 #optional
      - INCLUDE= #optional
      - EXCLUDE= #optional
      - TESTING_MODE=false #optional
      - PUSHOVER_TOKEN=<Pushover application API token>
      - PUSHOVER_RECIPIENT=<Pushover user and/or group key(s)>
    volumes:
      - qnap-pushover:/data
      - /etc/logs/event.log:/event.log:ro
    restart: always
```

### Docker CLI

```bash
docker volume create qnap-pushover
docker run -d \
  --name=qnap-pushover \
  -e TZ=America/New_York `#optional` \
  -e LOG_LEVEL=WARN `#optional` \
  -e NOTIFY_LEVEL=WARN `#optional` \
  -e POLL_INTERVAL=10 `#optional` \
  -e INCLUDE= `#optional` \
  -e EXCLUDE= `#optional` \
  -e TESTING_MODE=false `#optional` \
  -e PUSHOVER_TOKEN=<Pushover application API token> \
  -e PUSHOVER_RECIPIENT=<Pushover user and/or group key(s)> \
  -v qnap-pushover:/data \
  -v /etc/logs/event.log:/event.log:ro \
  --restart always \
  thecatlady/qnap-pushover
```

## Parameters

The container image is configured using the following parameters passed at runtime:

|Parameter|Function|Default Value|Required?|
|---|---|---|---|
|`-e TZ=`|[TZ database name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) of NAS time zone; e.g., `America/New_York`||no|
|`-e LOG_LEVEL=`|Container logging level; `DEBUG` < `INFO` < `WARN` < `ERROR` < `CRITICAL`|`WARN`|no|
|`-e NOTIFY_LEVEL=`|Minimum system event type to generate a notification; `INFO` < `WARN` < `ERROR`|`WARN`|no|
|`-e POLL_INTERVAL=`|Poll interval in seconds|`10`|no|
|`-e INCLUDE=`|List of keywords which must be present in the event description to trigger a notification (comma-delimited)||no|
|`-e EXCLUDE=`|List of keywords which must _not_ be present in the event description to trigger a notification (comma-delimited)||no|
|`-e TESTING_MODE=`|Testing mode (`true` or `false`); if set to `true`, will re-queue the last 10 system log events at _every_ container start and result in duplicate notifications|`false`|no|
|`-e PUSHOVER_TOKEN=`|[Pushover application API token](https://pushover.net/api#registration); e.g., `azGDORePK8gMaC0QOYAMyEEuzJnyUi`||**yes**|
|`-e PUSHOVER_RECIPIENT=`|[Pushover user and/or group key(s)](https://pushover.net/api#identifiers); e.g., `uQiRzpo4DXghDmr9QzzfQu27cmVRsG` or `gznej3rKEVAvPUxu9vvNnqpmZpokzF` (up to 50, comma-delimited)||**yes**|
|`-v qnap-pushover:/data`|Container data volume||**yes**|
|`-v /etc/logs/event.log:/event.log:ro`|QNAP event logs (mounted as read-only)||**yes**|
|`--restart`|Container [restart policy](https://docs.docker.com/engine/reference/run/#restart-policies---restart) (`always` or `unless-stopped` recommended)|`no`|no|

## Building Locally

If you would like to make modifications to the code, you can build the Docker image yourself instead of pulling the pre-built image available from [GitHub Container Registry (GHCR)](https://github.com/users/TheCatLady/packages/container/package/qnap-pushover) or [Docker Hub](https://hub.docker.com/r/thecatlady/qnap-pushover).

```bash
git clone https://github.com/TheCatLady/docker-qnap-pushover.git
cd docker-qnap-pushover
docker build \
  --no-cache \
  --pull \
  -t thecatlady/qnap-pushover .
docker volume create qnap-pushover
docker run -d \
  --name=qnap-pushover \
  -e TZ=America/New_York `#optional` \
  -e LOG_LEVEL=WARN `#optional` \
  -e NOTIFY_LEVEL=WARN `#optional` \
  -e POLL_INTERVAL=10 `#optional` \
  -e TESTING_MODE=false `#optional` \
  -e PUSHOVER_TOKEN=<Pushover application API token> \
  -e PUSHOVER_RECIPIENT=<Pushover user and/or group key(s)> \
  -v qnap-pushover:/data \
  -v /etc/logs/event.log:/event.log:ro \
  --restart always \
  thecatlady/qnap-pushover
```

## How to Contribute

Show your support by starring this project! &#x1F31F;  Pull requests, bug reports, and feature requests are also welcome!

You can also support me by [becoming a GitHub sponsor](https://github.com/sponsors/TheCatLady) or [making a one-time PayPal donation](http://paypal.me/DHoung) &#x1F496;