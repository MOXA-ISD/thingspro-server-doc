<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [ThingsPro Server (Development)](#thingspro-server-development)
  - [Infrastructure Setup](#infrastructure-setup)
    - [From scratch(Linux environment)](#from-scratchlinux-environment)
      - [Install utility](#install-utility)
        - [jq](#jq)
        - [Docker](#docker)
    - [Vagrantfile](#vagrantfile)
      - [Prerequisites](#prerequisites)
      - [Create infrastructure by Vagrant](#create-infrastructure-by-vagrant)
  - [Install ThingsProCS](#install-thingsprocs)
    - [1. ThingsProCS](#1-thingsprocs)
      - [Download from repo](#download-from-repo)
      - [Build from source code](#build-from-source-code)
    - [2. Run init script](#2-run-init-script)
    - [3. Start all service](#3-start-all-service)
    - [Note](#note)
  - [Update ThingsProCS](#update-thingsprocs)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ThingsPro Server (Development)

## Infrastructure Setup

### From scratch(Linux environment)

#### Install utility

##### jq

```shell
sudo apt-get update
sudo apt-get install jq
```

##### Docker

```shell
curl -fsSL get.docker.com -o /tmp/get-docker.sh
bash /tmp/get-docker.sh
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### Vagrantfile

#### Prerequisites

* [Vagrant](https://www.vagrantup.com/downloads.html)
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

#### Create infrastructure by Vagrant

Note: Please refer the [document](https://www.notion.so/isd/ffa5ff34bfc540cdb1deecb308518e5c#1859028574a242a09998ab0b768118b0) to find USER and PASSWORD.

```shell
$ mkdir workspace && cd workspace
$ wget http://download.moxa.online/devops-infrastructure/thingspro-cs-dev/Vagrantfile
$ vagrant up  # wait for a few minutes
$ vagrant ssh
```

## Install ThingsProCS

### 1. ThingsProCS

#### Download from repo

You can download ThingsProCS from http://repo.moxa.online/static/ThingsPro/Server For example:

```shell
$ wget http://repo.moxa.online/static/ThingsPro/Server/release/thingspro_v2.5/ThingsProCS-20180524-411.tar.gz
$ mkdir thingspro
$ tar xzvf ThingsProCS-20180524-411.tar.gz -C thingspro
```

#### Build from source code

You can git clone https://github.com/MOXA-ISD/thingspro-server under workspace folder on host or git clone into your environment. For example:

```shell
$ cd workspace
$ git clone https://github.com/MOXA-ISD/thingspro-server
$ vagrant up && vagrant ssh
$ cd thingspro-src
$ make clean && make release
$ tar xvf ThingsProCS_20180724-044058.tar.gz -C ../thingspro
```

### 2. Run init script

```shell
bash scripts/init-local-env.sh
```

### 3. Start all service

```shell
bash scripts/init-dev.sh
```

You can see the website http://yourip or https://yourip and use default account and password to login. If you using Vagrant to setup environment, please use http://localhost:8080 or https://localhost:8443


### Note

Generate Read/Write token on the web and copy it into `.env`, and also modify filebin path, then restart service

```shell
vim ${THINGSPRO_HOME}/.env
BASEURL=//{YOUR_SERVER_PUBLIC_IP}
MX_API_TOKEN=YOUR_READ_WRITE_TOKEN
FILEBIN_PATH=http://{YOUR_SERVER_PUBLIC_IP}/filebin
```
## Update ThingsProCS

### 1. ThingsProCS

#### Download from repo

You can download ThingsProCS from http://repo.moxa.online/static/ThingsPro/Server For example:
Note: please do not overwtire thingspro folder, please unzip to another folder, ex: thingspro2

```shell
$ wget http://repo.moxa.online/static/ThingsPro/Server/release/thingspro_v2.5/ThingsProCS-20180524-411.tar.gz
$ mkdir thingspro2
$ tar xzvf ThingsProCS-20180524-411.tar.gz -C thingspro
```

#### Import image, (under thingspro2)
```shell
$ cd thingspro2
$ make import-images
```

#### Modify thingspro/.env file
```shell
$ cd thingspro
$ vi .env
# modify the version to latest version
$ VERSION=20181018-481
```
