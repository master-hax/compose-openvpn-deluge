# compose-openvpn-deluge

a multi-container Docker application to run [Deluge](https://hub.docker.com/r/linuxserver/deluge) behind an [OpenVPN client](https://hub.docker.com/r/dperson/openvpn-client)

## how to set it up

1. download [docker-compose.yml](/docker-compose.yml)
1. put your `*.ovpn` file into `./openvpn`
1. run `docker-compose up`

if everything works correctly, Deluge should be running behind your VPN!

## how to use it

the Deluge web UI should be accessible at http://localhost:8112

if you want to use this persistently, you should probably
1. change the locations of the `deluge-data-volume` & `downloads-volume`
1. forward port 4001 with your VPN provider (or pick a different port)

## how it works

the `torrent-client` (Deluge) service shares the network stack of the `vpn-sidecar` service (Wireguard), which is tunneled through your VPN provider. to maintain local connectivity to the `torrent-client` container's web UI, we proxy to it to through the `web-proxy` service (Nginx) using [Docker container links](https://docs.docker.com/network/links/).
