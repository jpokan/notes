# Selfhosted-cgit index

This guide will show you how to install cgit on a RaspberryPi 2B.

##### Prerequisites:

- An installation of Raspberry Pi OS Lite (32-bit)

`(Released: 2022-04-04) A port of Debian Bullseye with no desktop environment`

- Upgrade libraries

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

- Have git installed.

```sh
$ apt install git
```

- Have a [SSL](ssl.md) certificate for your domain.
---

### Step 1

Install cgit from source. See this [guide](/web-development/selfhosted-cgit/cgit.md).

### Step 2

Setup nginx and fcgiwrap. Follow this [instructions](/web-development/selfhosted-cgit/nginx-server.md) to set it up.

--- tags ---
##### :index:
