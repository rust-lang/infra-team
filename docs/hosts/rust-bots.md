# rust-bots

* FQDN: `bots.infra.rust-lang.org` (behind the bastion)

## Common maintenance procedures

### Adding a new domain

First, edit `sudo vim /etc/nginx/nginx.conf` to edit the nginx configuration to add the domain.

```
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name <domain>.infra.rust-lang.org; # Edit <domain> to match here

    location /.well-known/acme-challenge {
        root /home/ssl-renew/challenges;
    }

    location / {
        # configure the domain here
    }
}
```

Then run `sudo -i -u ssl-renew vim renew.sh`. Add a `--domains` line to the script with the domain you're adding.

Then, run the script: `sudo -i -u ssl-renew ./renew.sh`
