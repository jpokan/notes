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

#### Install nginx
```
$ sudo apt install nginx
```

After installing nginx, your web server should be up and running. If you go to your RaspberryPi IP address, typing `192.168.1.x` in your browser, it should be serving you the nginx default website.
> `x` represents the address number of your device.
>
> To find your ip address in your RaspberryPi, type `ifconfig` from the terminal and check the `inet` value.

But..., this is only available from within our local area network or LAN. To make it accesible from anywhere else, we will need a few things:
- [x] A domain pointing to our PUBLIC IP address. 
- [x] Open some ports in our router to let traffic go through to our device.
- [x] Secure your traffic to the device with a SSL certificate. Follow [these instructions](/web-development/selfhosted-cgit/ssl.md).

Once you have all of the above we will create a new nginx web server configuration file called `cgit-server` inside `/etc/nginx/sites-available` with the following:
```
server {
    server_name your_domain.com;

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

Replace `your_domain` accordingly.

Tell nginx to enable our new site by creating a symbolic link of this file to the enabled sites folder (this is nginx specific). 
```
$ ln -s /etc/nginx/sites-available/cgit-server /etc/nginx/sites-enabled
```

Restart the nginx server.
```
$ service nginx restart
```

Since our configuration was made to point to port 8443, we can access it by going to `192.168.1.x:8443`.

Nginx service useful commands:
```
$ service nginx start
$ service nginx status
$ service nginx stop
$ service nginx restart
$ nginx -t                # test the configuration
```

#### Add a firewall

Install `ufw` and enable it to block all ports and enable only the ones you will use.

In this case, I will be allowing port 8443 since we added this port in our nginx server configuration.
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
