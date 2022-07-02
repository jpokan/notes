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
$ sudo certbot certonly --nginx --standalone --test-cert -d your_domain
```

> `--test-cert` this flag will help if the certification fails, because CA (Certificate Authority) service will block you after 6 failed certifications for 3 hours.
>
> `-d your_domain` replace this with the domain you want to certificate.
>
> `certonly` this will tell certbot to NOT modify our nginx server configuration

Enter your email, agree to the terms, don't agree to share email. Refer to this [article](https://landchad.net/basic/certbot/).

After this is done, you'll have the certificates in `/etc/letsencrypt/live/your_domain.com/`

##### reference links ---
- [certbot guide](https://certbot.eff.org/instructions?ws=nginx&os=debianbuster)

--- tags ---
##### :ssl: :server: :certbot:
