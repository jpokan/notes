# Selfhosted-cgit index

This guide will show you how to install cgit on a RaspberryPi 2B and use it as server so that you can access it anywhere. If you don't want to use a RaspberryPi, this guide should also work for any linux server with root access, just be aware of the differences that might be for your linux distro.

There's a lot of work that needs to be done before being able to serve cgit to the world like setting up a web server, port forwarding, getting a domain name, etc... That is why this guide is divided by topics but after this, you'll be able to have your own web server with cgit and understanding how to selfhost your own web server on a RaspberryPi 2B.

##### Prerequisites:

- A RaspberryPi 2B with an installation of Raspberry Pi OS Lite (32-bit)

`(Released: 2022-04-04) A port of Debian Bullseye with no desktop environment`

- Up to date libraries

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

- Have git installed.

```sh
$ apt install git
```

- Have a public IP address
- Have a domain or subdomain pointing to this IP address

---

### Step 1

Install cgit from source. See this [guide](/web-development/selfhosted-cgit/cgit.md).

### Step 2

Setup nginx and fcgiwrap. Follow this [instructions](/web-development/selfhosted-cgit/nginx-server.md) to set it up.

--- tags ---
##### :index:
