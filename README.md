# Whoogle Search

[![Latest Release](https://img.shields.io/github/v/release/benbusby/whoogle-search)](https://github.com/benbusby/shoogle/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.com/benbusby/whoogle-search.svg?branch=master)](https://travis-ci.com/benbusby/whoogle-search)
[![codebeat badge](https://codebeat.co/badges/e96cada2-fb6f-4528-8285-7d72abd74e8d)](https://codebeat.co/projects/github-com-benbusby-shoogle-master)

Get Google search results, but without any ads, javascript, AMP links, cookies, or IP address tracking. Easily deployable in one click as a Docker app, and customizable with a single config file. Quick and simple to implement as a primary search engine replacement on both desktop and mobile.

This ReadMe is stripped intentionally.
Refer to: [Benbusby/whoogle-search](https://github.com/benbusby/whoogle-search) and https://github.com/benbusby/whoogle-search/pull/21
This fork will not be updated and is only to demonstrate how to change the Dockerfile for armv7.

#### Docker CLI
```bash
git clone https://github.com/mohammedalqadi/whoogle-search
#git clone https://github.com/benbusby/whoogle-search (when PR#21 is implemented go back to original)
cd whoogle-search

#Change "FROM lsiobase/python:3.11" to "FROM lsiobase/python:arm32v7-3.11"
sed -i "1s/python:/python:arm32v7-/" Dockerfile

docker build --tag whooglesearch-armv7:1.0 .
docker run --publish 8888:5000 --detach --name whooglesearch whooglesearch-armv7:1.0
```

And kill with: `docker rm --force whooglesearch`

#### Alternatively
```bash
git clone https://github.com/sUyMur/whoogle-search-armv7
cd whoogle-search-armv7
docker build --tag whooglesearch-armv7:1.0 .
docker run --publish 8888:5000 --detach --name whooglesearch whooglesearch-armv7:1.0
```

And kill with: `docker rm --force whooglesearch`


#### Docker-Compose

```bash
version: '3.4'
services:
    whooglesearch:
        ports:             #Use when network_mode is not in use
            - '8888:5000'  # ^
        container_name: whooglesearch
        image: whooglesearch-armv7:1.0

        environment:
          - PUID=1000         #Otherwise config can't be changed?
          - PGID=1000         #Otherwise config can't be changed?
          - TZ=Europe/Berlin  #optional
          - UMASK_SET=022     #optional

#        network_mode: container:NordVPN               #optional

        healthcheck:                                  #optional
          test: ["CMD", "curl", "-f", "google.com"]   #optional
          interval: 1m                                #optional
          timeout: 5s                                 #optional
          retries: 5                                  #optional
          start_period: 15s                           #optional

        restart: unless-stopped
```
