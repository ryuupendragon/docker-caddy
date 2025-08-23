# Caddy

Custom Caddy Builds


Create CLOUDFLARE_API_TOKEN with below permissions.
 * Zone.Zone:Read
 * Zone.DNS:Edit

## [Cloudflare ACME](https://github.com/caddy-dns/cloudflare)

```Caddyfile
{
        email username@domain.com
        acme_dns cloudflare {$CLOUDFLARE_API_TOKEN} 
}
```

## [Dynamic DNS](https://github.com/mholt/caddy-dynamicdns)

```Caddyfile
{
        dynamic_dns {
                provider cloudflare {$CLOUDFLARE_API_TOKEN}
                domains {
                        example.com @ subdomain
                }
                check_interval 5m
                versions ipv4
                ttl 1m
        }
}
```

## [CrowdSec](https://github.com/hslatman/caddy-crowdsec-bouncer)

```Caddyfile
{
        crowdsec {
                api_url http://crowdsec:8080
                api_key {$CROWDSEC_API_KEY}
                ticker_interval 3s
                appsec_url http://crowdsec:7422
                #disable_streaming
                #enable_hard_fails
        }
}

#Add Logging to Caddy
(logging) {
        log {
                output file /logs/access.log {
                        roll_size 10MB # Create new file when size exceeds 10MB
                        roll_keep 5 # Keep at most 5 rolled files
                        roll_keep_for 336h # Delete files older than 14 days
                        roll_uncompressed
                }
        }
}

ryuu.in www.ryuu.in {
        import logging
        route {
                crowdsec
        }
        redir https://git.ryuu.in/ryuupendragon permanent
}

*.ryuu.in {
        import logging

        @a host a.ryuu.in
        handle @a {
                route {
                        crowdsec
                }
                reverse_proxy ip:port
        }

        @b host b.ryuu.in
        handle @b {
                route {
                        crowdsec
                }
                reverse_proxy ip:port
        }

        # Fallback for otherwise unhandled domains
        handle {
                route {
                        crowdsec
                }
                abort
        }
}

```


**Disclaimer**: Some inspirations were taken from [serfriz](https://github.com/serfriz/caddy-custom-builds)'s custom caddy builds for folder structure, parsing caddy version from Dockerfile, and readme. No code has been directly copied unless otherwise specified. This project is not affiliated with or endorsed by serfriz.
