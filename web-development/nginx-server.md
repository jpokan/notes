# Nginx web server on a Raspberry Pi 2B

ssl
certbot
nginx
ufw 
port forwarding

Install `ufw` and enable it to block all ports and enable only the ones you will use.

In this case, I will be allowing port 8443.

```sh
    $ apt install ufw
    $ ufw allow 8443
    $ ufw enable
```

##### reference links ---
[git server setup](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)

--- tags ---
##### :ssl: :rasbperrypi: :nginx: :ufw: :server:
