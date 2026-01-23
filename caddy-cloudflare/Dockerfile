FROM caddy:2.10.2-builder@sha256:6644af24bde2b4dbb07eb57637051abd2aa713e9787fa1eb544c3f31a0620898 AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare

FROM caddy:2.10.2@sha256:2adb640cdc0ce1d8870887c30af1e21edfb3cdfd8433431b2a15f40119a7d654

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
