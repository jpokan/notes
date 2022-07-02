# SSL instalation with certbot

Go to [certbot](https://certbot.eff.org/instructions?ws=nginx&os=debianbuster) and select nginx on debian 10 because we are going to install it on a RaspberryPi 2B mentioned in our [prerequisites](index.md).

Install snapd on debian, you can check this [guide](https://snapcraft.io/docs/installing-snap-on-debian) for reference.

```
$ sudo apt update
$ sudo apt install snapd
```

Check if snapd is up to date.

```
$ sudo snap install core
$ sudo snap refresh core
```

Install certbot

```
$ sudo snap install --classic certbot
```

Ensure that `certbot` command can be run creating with a symbolic link.

```
sudo ln -s /snap/bin/certbot /usr/bin/cerbot
```
For our setup we will install a standalone version certificate. Refer to this [article](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-debian-10).

```
$ sudo certbot certonly --standalone --test-cert -d your_domain
```

> `--test-cert` use this flag to make a staging certificate first, CA (Certificate Authority) service will block you after 6 failed certifications for 3 hours.
>
> `-d your_domain` replace this with the domain you want to certify.
>
> `certonly` this will tell certbot to NOT modify our nginx server configuration.

Enter your email, agree to the terms, don't agree to share email.

After this is done, you'll have the certificates in `/etc/letsencrypt/live/your_domain.com/`

If you get an error about port 80 connection similar to this:

```
The Certificate Authority failed to download the challenge files from the temporary standalone webserver started by Certbot on port 80. Ensure that the listed domains point to this machine and that it can accept inbound connections from the internet.
```
Make sure that you have opened port 80 in your router settings. Learn about [port forwarding](port-forwarding.md).

If you already have port 80 opened and get another error like this:

```
Could not bind TCP port 80 because it is already in use by another process on
this system (such as a web server). Please stop the program in question and then
try again.
```

This is because the default nginx server configuration uses port 80 to serve a default web page, one way we fix this is by changing the default port to another one like port 8000. Go to `/etc/nginx/sites-available/` and edit the `default` file inside.

```
...
# Default server configuration
#
server {
        listen 8000 default_server;
        listen [::]:8000 default_server;

        # SSL configuration
...
```

When it is successful, the following message will show:
```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/your_domain.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/your_domain.com/privkey.pem
This certificate expires on 2022-09-30.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

```

Because we have used the `--test-cert` flag, our certificate is on a (STAGING) state. To fix this just run the same command but without this flag.
```
$ sudo certbot certonly --standalone -d your_domain
```

Now that we have our production certificate we can go back and add them to our [nginx-server](nginx-server.md) configuration.

##### reference links ---
- [certbot official guide](https://certbot.eff.org/instructions?ws=nginx&os=debianbuster)
- [certbot guide 2](https://landchad.net/basic/certbot/)
 
--- tags ---
##### :ssl: :server: :certbot: :error: :snapd:
