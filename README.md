# OpenCanary
Thinkst Applied Research

![opencanary logo](docs/logo.png)

## Overview

In essence, OpenCanary creates a network honeypot allowing you to catch hackers before they fully compromise your systems. As a technical definition, OpenCanary is a daemon that runs several canary versions of services that alerts when a service is (ab)used.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [Prerequisites](#prerequisites)
- [Features](#features)
- [Installation on Ubuntu](#installation-ubuntu)
- [Installation on OS X](#installation-os-x)
- [Installation using Git](#installation-git)
- [Running OpenCanary](#running-opencanary)
- [Samba setup for SMB service](#samba-setup-optional)
- [Docker Compose Usage](#docker-compose)
- [Docker Usage](#docker)
- [FAQ](#faq)
- [Contributing](#contributing)

## Code of Conduct

This project and everyone participating in it is governed by the
[Code of Conduct](https://github.com/thinkst/.github/blob/master/CODE_OF_CONDUCT.md).
By participating, you are expected to uphold this code. Please report unacceptable behavior
to github@thinkst.com.

## Prerequisites

* Python 3.7 (Recommended Python 3.7+)
* [Optional] SNMP requires the Python library scapy
* [Optional] Samba module needs a working installation of samba
## Features

* Mimic an array of network-accessible services for attackers to interact with.
* Receive various alerts as soon as potential threats are detected, highlighting the threat source IP address and where the breach may have occurred.

NOTE: new feature requests are tracked [here](https://github.com/thinkst/opencanary/discussions/categories/feature-requests)

## Installation [UBUNTU]

For updated and cleaner documentation, please head over to http://opencanary.org

Installation on Ubuntu 20.04:

```
$ sudo apt-get install python3-dev python3-pip python3-virtualenv python3-venv python3-scapy libssl-dev libpcap-dev
$ sudo apt install samba # if you plan to use the smb module
$ virtualenv env/
$ . env/bin/activate
$ pip install opencanary
$ pip install scapy pcapy # optional
```
## Installation [OS X]

Installation OS X needs an extra step, as multiple OpenSSL versions
may exist, which confounds the Python libraries using it.

```
$ virtualenv env/
$ . env/bin/activate
```

Macports users should then run:
```
$ sudo port install openssl
$ env ARCHFLAGS="-arch x86_64" LDFLAGS="-L/opt/local/lib" CFLAGS="-I/opt/local/include" pip install cryptography
```

Alternatively, homebrew users run:

If Macbook is x86:
````
$ brew install openssl
$ env ARCHFLAGS="-arch x86_64" LDFLAGS="-L/usr/local/opt/openssl/lib" CFLAGS="-I/usr/local/opt/openssl/include" pip install cryptography
````

if Macbook is M1:
```
$ brew install openssl
$ env ARCHFLAGS="-arch arm64" LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib" CFLAGS="-I/opt/homebrew/opt/openssl@1.1/include" pip install cryptography
```

Now the installation can run as usual:
```
$ pip install opencanary
$ pip install scapy pcapy # optional
```
## Installation [GIT]

To install from source, instead of running pip do the following:

```
$ git clone https://github.com/thinkst/opencanary
$ cd opencanary
$ python setup.py sdist
$ cd dist
$ pip install opencanary-<version>.tar.gz
```

## Running OpenCanary

Please note that for the Portscan service, we have added a `portscan.ignore_localhost` setting, which means the Opencanary `portscan` service will ignore (not alert on) port scans originating for the localhost IP (`127.0.0.1`). This setting is false by default.

OpenCanary is started by running:

```
$ . env/bin/activate
$ opencanaryd --start
```

On the first run, instructions are printed to get to a working config.

```
$ opencanaryd --copyconfig
```

Which will create a folder, `/etc/opencanaryd` and a config file inside that folder, `opencanary.conf`. You must now edit the config file to determine which services and logging options you want to enable.

When OpenCanary starts it looks for config files in the following order:

1. ./opencanary.conf (i.e. the directory where OpenCanary is installed)
2. ~/.opencanary.conf (i.e. the home directory of the user, usually this will be root so /root/.opencanary.conf)
3. /etc/opencanaryd/opencanary.conf

It will use the first config file that exists.

## Samba Setup (optional)

This is required for the `smb` module.

Head over to our step-by-step wiki over [here](https://github.com/thinkst/opencanary/wiki/Opencanary-and-Samba)

## Docker Compose

> Requires [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed.

NOTE: The portscan module is automatically disabled for Dockerised OpenCanary.

1. Edit the `data/.opencanary.conf` file to enable, disable or customize the services that will run.

1. Edit the `ports` section of the `docker-compose.yml` file to enable/disable the desired ports based on the services you have enabled in the config file.

1. Build and run the container.

    To run the latest Docker image (based on the code on a given branch) run:
    ```bash
    docker-compose up -d --build latest
    ```
    To run a Docker image based on what has been released in Pypi, run:
    ```bash
    docker-compose up -d --build stable
    ```

> To view the logs run `docker-compose logs latest` or `docker-compose logs stable`

> To stop the container run `docker-compose down`

## Docker

> Requires [Docker](https://docs.docker.com/get-docker/) installed.

NOTE: The portscan module is automatically disabled for Dockerised OpenCanary.

1. Edit the `data/.opencanary.conf` file to enable, disable or customize the services that will run.

1. Build a Docker image to run.

    To build the latest Docker image (based on the code on a given branch) run:

    ```bash
    docker build -t opencanary -f Dockerfile.latest .
    ```

    To build a Docker image based on what has been released in Pypi, run:

    ```bash
    docker build -t opencanary -f Dockerfile.stable .
    ```

1. Run the docker image with the following command:

```bash
# You will need to add/remove the ports you are using by listing them with `-p ##:##`
docker run --rm --detach -p 21:21 -p 80:80 -v "${PWD}/data/.opencanary.conf":"/root/.opencanary.conf" --name opencanary opencanary
```

> To view the logs run `docker logs opencanary`

> To stop the container, run `docker stop opencanary`

## FAQ

We have a FAQ over [here](https://github.com/thinkst/opencanary/wiki)

## Contributing

Please check out our [Code of Conduct](https://github.com/thinkst/.github/blob/master/CODE_OF_CONDUCT.md) and [Contributing](https://github.com/thinkst/.github/blob/master/CONTRIBUTING.md) documents before submitting a pull request.

We look forward to your valuable contributions.
