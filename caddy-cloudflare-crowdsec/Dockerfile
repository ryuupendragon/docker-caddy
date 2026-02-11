FROM caddy:2.10.2-builder@sha256:23e70f63a88bacd9b0564d055eed5c25b98e90930876cca01aee1f5d1ae29748 AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/hslatman/caddy-crowdsec-bouncer/http \
    --with github.com/hslatman/caddy-crowdsec-bouncer/layer4 \
    --with github.com/hslatman/caddy-crowdsec-bouncer/appsec

FROM caddy:2.10.2@sha256:c3d7ee5d2b11f9dc54f947f68a734c84e9c9666c92c88a7f30b9cba5da182adb

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
