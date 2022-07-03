# SSL instalation with certbot

If you want the original guide go to [certbot](https://certbot.eff.org/instructions?ws=nginx&os=debianbuster). In there you could choose another type of server and operating system if you're not going to install it on a RaspberryPi 2B.

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

The next command will call certbot to create a test certificate for your domain.
```
$ sudo certbot certonly --standalone --test-cert -d your_domain
```

> `--standalone` this means that certbot will run its built-in server to authenticate the request of the certificate.
>
> `--test-cert` use this flag to make a staging certificate first, CA (Certificate Authority) service will block you after 6 failed certifications for 3 hours.
>
> `-d your_domain` replace this with the domain you want to certify.
>
> `certonly` this will tell certbot to NOT modify our nginx server configuration.

Enter your email, agree to the terms, don't agree to share email.

After this is done, you'll have the certificates in `/etc/letsencrypt/live/your_domain.com/`

If you get an error about port 80 connection similar to this:
```
The Certificate Authority failed to download the challenge files from the
temporary standalone webserver started by Certbot on port 80.
Ensure that the listed domains point to this machine and that it can accept
inbound connections from the internet.
```

This means that the port 80 is not open in your router. To fix this, read how to [port forward](port-forwarding.md).

If you already have port 80 open and get another error like this:
```
Could not bind TCP port 80 because it is already in use by another process on
this system (such as a web server). Please stop the program in question and then
try again.
```

This is because when we started the nginx server, it is running a default web server on port 80. One way to fix this is by changing the default server port to another one like port 8000. Go to `/etc/nginx/sites-available/` and edit the `default` file.
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

Then restart the nginx server and run the certification command again:
```
$ service nginx restart
$ sudo certbot certonly --standalone --test-cert -d your_domain
```

If it is successful, the following message will show:
```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/your_domain.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/your_domain.com/privkey.pem
This certificate expires on 2022-09-30.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.
```

Certbot will save some of the information you used to create the certificate and will use that to renew or autorenew it next time.
To test if certbot can renew your certificate run:
```
$ sudo certbot renew --dry-run
```

Now that we know that everything went good, we can remove the `--test-cert` flag, to make a production certificate. I'd recommend you run this only when you're sure about that everything is working.
```
$ sudo certbot certonly --standalone -d your_domain
```

Now that we have our production certificate we can go back and add them to our [nginx-server](nginx-server.md) configuration.


##### reference links ---
- [certbot official guide](https://certbot.eff.org/instructions?ws=nginx&os=debianbuster)
- [certbot guide 2](https://landchad.net/basic/certbot/)
- [standalone mode](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-debian-10). 
- [dns validation](https://www.digitalocean.com/community/tutorials/how-to-acquire-a-let-s-encrypt-certificate-using-dns-validation-with-acme-dns-certbot-on-ubuntu-18-04) - another way to create a SSL cert.

--- tags ---
##### :ssl: :server: :certbot: :error: :snapd:
