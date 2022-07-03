# Nginx web server on a RaspberryPi 2B

Because we want to use nginx to serve Cgit which uses CGI, we will need to add a wrapper around nginx. Bryan Brattlof explains it much better [here](https://bryanbrattlof.com/cgit-nginx-gitolite-a-personal-git-server/).

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

After installing nginx, your web server should be up and running. Type the IP address of your RaspberryPi `192.168.1.x` in your browser, it should be serving you the nginx default website.
> `x` represents the address number of your device.
>
> To find the IP address of your RaspberryPi, type `ifconfig` in the terminal and check the `inet` value.

But..., this is only available from within our local area network or LAN. To make it accesible from anywhere else, we will need a few things:
- [x] A domain pointing to our PUBLIC IP address. See [how](domain.md).
- [x] Open ports in our router to let traffic go through and to our RaspberryPi. See [port forwarding](port-forwarding.md).
- [x] Secure your traffic with a SSL certificate. Follow [these instructions](/web-development/selfhosted-cgit/ssl.md).

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

Replace `your_domain` accordingly in the `server_name` and `ssl_certificate/_key`.

We will also need to tell nginx to enable our new site. To do this, create a symbolic link of this file to the directory where nginx looks for the enabled sites (this is nginx specific). 
```
$ ln -s /etc/nginx/sites-available/cgit-server /etc/nginx/sites-enabled
```

In order for nginx to know about the new site, we must restart the server.
```
$ service nginx restart
```

Since our configuration was made to point to port 8443, we can access it by going to `https://192.168.1.x:8443`.

Nginx service useful commands:
```
$ service nginx start
$ service nginx status
$ service nginx stop
$ service nginx restart
$ nginx -t                # test the configuration
```

#### Add a firewall

To add an extra layer of security to our RaspberryPi or server, we can use a firewall.
We can do this by installing `ufw` and enable it to block all ports and tell it to use some ports.
In this case, I will be allowing port 22(ssh), 80(http) and 8443(https) since we added those ports in our router and in our nginx server configuration.
```sh
$ sudo apt install ufw
$ sudo ufw allow ssh
$ sudo ufw allow 80/tcp
$ sudo ufw allow 8443/tcp
$ sudo ufw enable
```

Tip: you can use `$ sudo ufw verbose` to check which ports have been enabled.

This is all for now, but you can do much more with nginx.

##### reference links ---
- [git server setup](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)
- [https nginx configuration](https://nginx.org/en/docs/http/configuring_https_servers.html)

--- tags ---
##### :ssl: :raspberrypi: :server: :nginx: :ufw:
