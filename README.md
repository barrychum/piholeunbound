# piholeunbound

Example .env file in the same directory as your docker-compose.yaml file:

ServerIP=192.168.1.10
TZ=Asia/Taipei
WEBPASSWORD=QWERTY123456asdfASDF
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=192.168.1.1
REV_SERVER_CIDR=192.168.0.0/16
HOSTNAME=pihole
DOMAIN_NAME=pihole.local

================================

version: '2'

volumes:
  etc_pihole-unbound:
  etc_pihole_dnsmasq-unbound:

services:
  pihole:
    container_name: pihole
    image: cbcrowe/pihole-unbound:latest
    hostname: ${HOSTNAME}
    domainname: ${DOMAIN_NAME}
    ports:
      - 443:443/tcp
      - 53:53/tcp
      - 53:53/udp
      - 80:80/tcp
      # - 5335:5335/tcp # Uncomment to enable unbound access on local server
      # - 22/tcp # Uncomment to enable SSH
    environment:
      ServerIP: ${ServerIP}
      TZ: ${TZ}
      WEBPASSWORD: ${WEBPASSWORD}
      REV_SERVER: ${REV_SERVER}
      REV_SERVER_TARGET: ${REV_SERVER_TARGET}
      REV_SERVER_DOMAIN: ${REV_SERVER_DOMAIN}
      REV_SERVER_CIDR: ${REV_SERVER_CIDR}
      DNS1: 127.0.0.1#5335 # Hardcoded to our Unbound server
      DNS2: 127.0.0.1#5335 # Hardcoded to our Unbound server
      DNSSEC: "true" # Enable DNSSEC
    volumes:
      - etc_pihole-unbound:/etc/pihole:rw
      - etc_pihole_dnsmasq-unbound:/etc/dnsmasq.d:rw
    restart: unless-stopped


