[EFAULT] Failed 'down' action for 'qbit-vpn' app. Please check /var/log/app_lifecycle.log for more details

services:
  bazarr:
    container_name: bazarr
    environment:
      - PUID=568
      - PGID=1000
      - TZ=America/New_York
    image: lscr.io/linuxserver/bazarr:latest
    ports:
      - '6767:6767'
    restart: unless-stopped
    volumes:
      - /mnt/media_server/apps/bazarr:/config
      - /mnt/media_server/downloads:/downloads
      - /mnt/mnt/shows:/shows
  gluetun:
    cap_add:
      - NET_ADMIN
    container_name: wireguard_vpn
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER= custom
      - VPN_TYPE=wireguard
      - FIREWALL_OUTBOUND_SUBNETS=192.168.1.0/24
      - WIREGUARD_ENDPOINT_IP=129.80.162.199
      - WIREGUARD_ENDPOINT_PORT=51820
      - WIREGUARD_PUBLIC_KEY=Omm1xdclctMDPUCRJwFUSsbuOf0cyLd2NvnLAEn/n0o=
      - WIREGUARD_PRIVATE_KEY=wKufQ+t0yqC15FvIjFJ22sYMvmunkJAsN6WKrusI0FY=
      - WIREGUARD_PRESHARED_KEY=QBOl03pQCXat9vb9W63EUiNG65jFWAd4e4F1nVTs6uQ=
      - WIREGUARD_ADDRESSES=10.66.66.2/32
    image: qmcgaw/gluetun:latest
    ports:
      - 8099:8080/tcp
      - 6881:6881/tcp
      - 6881:6881/udp
    restart: unless-stopped
  prowlarr:
    container_name: prowlarr
    environment:
      - PUID=568
      - PGID=1000
      - TZ=America/New_York
    image: lscr.io/linuxserver/prowlarr:latest
    ports:
      - '9696:9696'
    restart: unless-stopped
    volumes:
      - /mnt/media_server/apps/prowlarr:/config
      - /mnt/mnt/shows:/shows
  qbittorrent:
    container_name: qbittorrent
    depends_on:
      - gluetun
    environment:
      - PUID=568
      - PGID=1000
      - TZ=America/New_York
      - WEBUI_PORT=8080
    image: lscr.io/linuxserver/qbittorrent:latest
    network_mode: service:gluetun
    restart: unless-stopped
    volumes:
      - /mnt/media_server/apps/qbittorrent:/config
      - /mnt/mnt/downloads2:/downloads2
      - /mnt/mnt/shows:/shows
  sonarr:
    container_name: sonarr
    environment:
      - PUID=568
      - PGID=1000
      - TZ=America/New_York
    image: lscr.io/linuxserver/sonarr:latest
    ports:
      - '8989:8989'
    restart: unless-stopped
    volumes:
      - /mnt/media_server/apps/sonarr:/config
      - /mnt/mnt/downloads2:/downloads2
      - /mnt/mnt/shows:/shows
  flaresolverr:
    container_name: flaresolverr
    image: ghcr.io/flaresolverr/flaresolverr:latest
    environment:
      - LOG_LEVEL=info
      - TZ=America/New_York
    ports:
      - '8191:8191'
    restart: unless-stopped