FROM caddy:2.11.4-builder@sha256:26f5271c26850df54ef7a9f3704b5d688c1ad51b5cde509c9d011b4e95cc8397 AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/mholt/caddy-l4/modules/l4proxy \
    --with github.com/hslatman/caddy-crowdsec-bouncer/http \
    --with github.com/hslatman/caddy-crowdsec-bouncer/layer4 \
    --with github.com/hslatman/caddy-crowdsec-bouncer/appsec

FROM caddy:2.11.4@sha256:cb9d71ad83182011b79355cd57692686374bd78d6fe327efe0ff8507da03ab13

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
