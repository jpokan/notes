# Cgit on a RaspberryPi 2B

Read more about [git-server](git-server.md). Skip if you already have experience with git server.

#### Clone Cgit

```
$ mkdir ~/src
$ cd src
$ git clone https://git.zx2c4.com/cgit
```

#### Install Cgit dependencies

```
$ apt install libc6 liblua5.1-0 zlib1g \
  python3-docutils python3-markdown python3-pygments
```

#### Include git submodule

```
$ cd cgit
$ git submodule init
$ git submodule update
```

#### Build Cgit

We will need to create a `cgit.conf` to customize our building of cgit.
```
$ vim cgit.conf
```

and add the following

```
CGIT_SCRIPT_PATH = /var/www/html/cgit/cgi
CGIT_CONFIG = /var/www/html/cgit/cgitrc
CACHE_ROOT = /var/www/html/cgit/cache
prefix = /var/www/html/cgit
libdir = $(prefix)
filterdir = $(libdir)/filters
```

Make the build and install it
```
$ make
$ make install
```

If you get an error during this step complaining about openssl, you will likely have to install the development package. Check [this](https://stackoverflow.com/questions/3016956/how-do-i-install-the-openssl-libraries-on-ubuntu/3016986#3016986) and run `$ sudo apt-get install libssl-dev`

To test if it worked.

```
$ ./cgit
Content-Type: text/html; charset=UTF-8
Last-Modified: Fri, 7 Jun 2022 20:34:43 GMT
Expires: Fri, 7 Jun 2022 20:44:43 GMT

<!DOCTYPE html>
```

To customize Cgit, create a `cgitrc` file in `/var/www/html/cgit/cgitrc`.

See [manual](https://git.zx2c4.com/cgit/tree/cgitrc.5.txt) to configure this file.

##### reference links ---
- [cgit](https://git.zx2c4.com/cgit/about/)
- [cgit guide](https://bryanbrattlof.com/cgit-nginx-gitolite-a-personal-git-server/)
- [cgit guide 2](https://landchad.net/cgit/)

--- tags ---
##### :rasbperrypi: :cgit: :git:
