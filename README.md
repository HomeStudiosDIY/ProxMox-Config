# ProxMox-Config

Here is some settings and how to's to get your proxmox server setting done!!  

I will also have all my LXC config and the resons why I have done what I have done!!!


# Connect to your NAS with NFS 

nano /etc/fstab

10.0.0.1:/volume1/Stream/ /mnt/data/stream nfs defaults 0 0  
10.0.0.1:/volumeUSB1/usbshare /mnt/data/usb nfs defaults 0 0  
10.0.0.1:/volume1/Photos-Link /mnt/data/photos nfs defaults 0 0  
10.0.0.1:/volume1/Downloads /mnt/data/downloads nfs defaults 0 0  



Reload systemd: systemctl daemon-reload  
Mount shares: mount -a  


# NVIDIA Drivers Install





# Opensence

# UniFi


	
mkdir /data
mkdir /data/unifi	


nano /data/unifi/init-mongo.sh



#!/bin/bash

if which mongosh > /dev/null 2>&1; then
  mongo_init_bin='mongosh'
else
  mongo_init_bin='mongo'
fi
"${mongo_init_bin}" <<EOF
use ${MONGO_AUTHSOURCE}
db.auth("${MONGO_INITDB_ROOT_USERNAME}", "${MONGO_INITDB_ROOT_PASSWORD}")
db.createUser({
  user: "${MONGO_USER}",
  pwd: "${MONGO_PASS}",
  roles: [
    { db: "${MONGO_DBNAME}", role: "dbOwner" },
    { db: "${MONGO_DBNAME}_stat", role: "dbOwner" }
  ]
})
EOF	
	






services:
  unifi-db:
    image: docker.io/mongo
    container_name: unifi-db
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=unifi
      - MONGO_USER=unifi
      - MONGO_PASS=unifi
      - MONGO_DBNAME=unifi
      - MONGO_AUTHSOURCE=admin
    volumes:
      - /data/unifi/db:/data/db
      - /data/unifi/init-mongo.sh:/docker-entrypoint-initdb.d/init-mongo.sh:ro
    restart: unless-stopped


  unifi-network-application:
    image: lscr.io/linuxserver/unifi-network-application:latest
    container_name: unifi-network-application
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MONGO_USER=unifi
      - MONGO_PASS=unifi
      - MONGO_HOST=unifi-db
      - MONGO_PORT=27017
      - MONGO_DBNAME=unifi
      - MONGO_AUTHSOURCE=admin
      #- MEM_LIMIT=1024 #optional
      #- MEM_STARTUP=1024 #optional
      #- MONGO_TLS= #optional
    volumes:
      - /data/unifi/config:/config
    ports:
      - 8443:8443
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      #- 1900:1900/udp #optional
      #- 8843:8843 #optional
      #- 8880:8880 #optional
      #- 6789:6789 #optional
      #- 5514:5514/udp #optional
    restart: unless-stopped
	
	



# Vaultwarden

# Home Assistant

# Jellefin Setup and Config

# Plex Setup and Config



