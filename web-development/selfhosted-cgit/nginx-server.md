# Nginx web server on a RaspberryPi 2B

#### Install FastCGI Wrapper
```
$ sudo apt install fcgiwrap
```

Enable and start the service
```
$ systemctl enable fcgiwrap
$ systemctl start fcgiwrap
```

This configuration assumes you already have a SSL certificate installed. If you haven't, consider getting one with [certbot](https://certbot.eff.org/).
I followed [these instructions](/web-development/selfhosted-cgit/ssl.md).

#### Install nginx
```
$ sudo apt install nginx
```

Then inside`/etc/nginx/sites-available` create a file with this:
```
server {
    server_name  git.your_domain.com;

    listen [::]:8443 ssl;
    listen 8443 ssl;

    ssl_certificate /etc/letsencrypt/live/your_domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your_domain.com/privkey.pem;

    access_log  /var/log/nginx/cgit-access.log;
    error_log   /var/log/nginx/cgit-error.log;

    root /var/www/html/cgit/cgi;
    try_files $uri @cgit;

    location @cgit {
        include          fastcgi_params;
        fastcgi_param    SCRIPT_FILENAME /var/www/html/cgit/cgi/cgit.cgi;
        fastcgi_pass     unix:/run/fcgiwrap.socket;

        fastcgi_param    PATH_INFO    $uri;
        fastcgi_param    QUERY_STRING $args;
        fastcgi_param    HTTP_HOST    $server_name;
    }
}
```

#### Add a firewall
Install `ufw` and enable it to block all ports and enable only the ones you will use.

In this case, I will be allowing port 8443.
```sh
$ apt install ufw
$ ufw allow 8443
$ ufw enable
```

##### reference links ---
- [git server setup](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)
- [https nginx configuration](https://nginx.org/en/docs/http/configuring_https_servers.html)

--- tags ---
##### :ssl: :raspberrypi: :server: :nginx: :ufw:
