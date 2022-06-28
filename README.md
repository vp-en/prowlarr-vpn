# Documentation

An overview of the docker images can be found here:


`https://github.com/users/vp-en/packages/container/package/prowlarr-vpn`

You can pull from either the nightly branch, the pr branch (pull requests) and the testing branch (the official Prowlarr releases)
`docker pull ghcr.io/vp-en/prowlarr-vpn:testing`

`docker pull ghcr.io/vp-en/prowlarr-vpn:nightly`

`docker pull ghcr.io/vp-en/prowlarr-vpn:pr`

They all include Wireguard support.

Privoxy is not supported (as of now), which means the environment setting `PRIVOXY_ENABLED` does nothing in this image.

If you need a tutorial how to set it up, you can use the same instructions from [Hotio's qBittorrent page](https://hotio.dev/containers/qbittorrent/#wireguard-vpn-support).



# Docker Compose
My docker compose looks like this:
```
version: "3.9"

services:
  prowlarr:
    container_name: prowlarr
    image: ghcr.io/vp-en/prowlarr-vpn:testing
    ports:
      - "9696:9696"
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Europe/Copenhagen
      - VPN_ENABLED=true
      - VPN_CONF=wg0
      - VPN_IP_CHECK_DELAY=5
    volumes:
      - /your/local/path:/config
    cap_add:
      - NET_ADMIN
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
    restart: unless-stopped
```

If you get errors regarding `ip6table_filter`, you should probably disable ipv6 by setting `net.ipv6.conf.all.disable_ipv6=0` to `1`.


# Credits
All credits goes to [hotio.dev](https://hotio.dev)!
You can use the Hotio [discord](https://hotio.dev/discord) server for support.