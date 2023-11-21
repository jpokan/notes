# Port forwarding

⚠️ Proceed with caution ⚠️
> Port forwarding to your home network is not recommended, you should be very careful and secure this as much as possible.

> Also think if you're going to have a lot of incomming traffic because it will eat up your internet bandwidth and since it is connected to your own private home network, you should be wary for hackers and attacks. If you're concerned about security you can watch [this](https://www.youtube.com/watch?v=ZhMw53Ud2tY) for tips on how to secure your server.
---

Why we need to port forward and which ports?

- We need to port forward port 80 for the http-01 challenge to create our SSL certificates, if your ISP blocks this port then you won't be able to complete the challenge and you won't get the certificates. See more about [challenge types](https://letsencrypt.org/docs/challenge-types/#http-01-challenge).

- We need to port forward port 443 to access our web server with a clean URL like https://your-domain.com. Again, if it is blocked by your ISP, you can still make it work but with another port number like 8443 and your URL will be like this https://your-domain.com:8443.

Anyways, to let the outside world access our local area network we need to open some specific ports.

Go to your router admin page, usually typing `192.168.1.1` in your browser. Log in and go to the Port Forward section under firewall tab.
> ISPs or internet service providers might block this. If you can't log in, you should contact them and ask them for the username and password.

In my case, I will enable port 80 to let certbot create the SSL certificate.
And port 8443 for our nginx server that only accepts secure (HTTPS) requests.

| Name    | Protocol | IP Address  | Public Port | Private Port |
|---------|----------|-------------|-------------|--------------|
| certbot | TCP      | 192.168.1.x | 80          | 80           |
| git-srv | TCP      | 192.168.1.x | 8443        | 8443         |

Remember, port 80 for SSL certificate creation and renewal. And port 443 or any other port for the secure connection with HTTPS enabled.

Go back to [nginx server](nginx-server.md).


##### reference links ---

--- tags ---
##### :port forward: :ssl:

