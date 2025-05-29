# ProxMox-Config

Here is some settings and how to's to get your proxmox server setting done!!  

I will also have all my LXC config and the resons why I have done what I have done!!!

![image](https://github.com/user-attachments/assets/a60fffb1-2eb3-4f06-be77-1b8c25a4af98)


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


	



# Vaultwarden

# Home Assistant

# Jellefin Setup and Config

# Plex Setup and Config





Proxmox installer
nomodeset









mkdir /mnt/data
mkdir /mnt/data/stream
mkdir /mnt/data/usb
mkdir /mnt/data/photos


mkdir /mnt/pve/disk4tb/frigate



mkdir /mnt/pve/disk4tb/downloads



Connect to NFS 
nano /etc/fstab

10.0.0.1:/volume1/Stream/ /mnt/data/stream nfs defaults 0 0
10.0.0.1:/volumeUSB1/usbshare /mnt/data/usb nfs defaults 0 0
10.0.0.1:/volume1/Photos-Link /mnt/data/photos nfs defaults 0 0
10.0.0.1:/volume1/Downloads /mnt/data/downloads nfs defaults 0 0



Reload systemd: systemctl daemon-reload
Mount shares: mount -a



chown 100109:100117 /mnt/pve/disk4tb/frigate/




NVIDIA

apt update && apt upgrade -y && apt install pve-headers build-essential software-properties-common make nvtop htop -y
update-initramfs -u



wget https://uk.download.nvidia.com/XFree86/Linux-x86_64/550.142/NVIDIA-Linux-x86_64-550.142.run


chmod +x NVIDIA-Linux-x86_64-550.144.03.run


./NVIDIA-Linux-x86_64-550.144.03.run --dkms


ls -al /dev/nvidia*






LXC Setup for Nvida 


pct push 105 NVIDIA-Linux-x86_64-550.144.03.run /root/NVIDIA-Linux-x86_64-550.144.03.run

chmod +x NVIDIA-Linux-x86_64-550.144.03.run

./NVIDIA-Linux-x86_64-550.144.03.run --no-kernel-modules

apt install gpg curl
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

apt update
apt install nvidia-container-toolkit

nvidia-smi

nvidia-ctk runtime configure --runtime=docker




nano /etc/nvidia-container-runtime/config.toml
#no-cgroups = false
to
no-cgroups = true

nano /etc/pve/lxc/105.conf



lxc.cgroup2.devices.allow: c 195:* rwm
lxc.cgroup2.devices.allow: c 234:* rwm
lxc.cgroup2.devices.allow: c 237:* rwm
lxc.mount.entry: /dev/nvidia0 dev/nvidia0 none bind,optional,create=file
lxc.mount.entry: /dev/nvidiactl dev/nvidiactl none bind,optional,create=file
lxc.mount.entry: /dev/nvidia-modeset dev/nvidia-modeset none bind,optional,create=file
lxc.mount.entry: /dev/nvidia-uvm dev/nvidia-uvm none bind,optional,create=file
lxc.mount.entry: /dev/nvidia-uvm-tools dev/nvidia-uvm-tools none bind,optional,create=file
lxc.mount.entry: /dev/nvidia-caps/nvidia-cap1 dev/nvidia-caps/nvidia-cap1 none bind,optional,create=file
lxc.mount.entry: /dev/nvidia-caps/nvidia-cap2 dev/nvidia-caps/nvidia-cap2 none bind,optional,create=file


RamDisk:
lxc.mount.entry: tmpfs dev/shm tmpfs size=4G,nosuid,nodev,noexec,create=dir 0 0





FRIGATE:
 To update Frigate, create a new container and transfer your configuration.


mkdir /data/
mkdir /data/config/
mkdir /data/cctv/


pct set 104 -mp0 /mnt/pve/disk4tb/frigate,mp=/data/cctv/




nano /etc/pve/lxc/104.conf

lxc.mount.entry: /dev/bus/usb/002/ dev/bus/usb/002/ none bind,optional,create=dir 0,0







IMMICH:
immich

mkdir /data
mkdir /data/immich
mkdir /data/photos

pct set 106 -mp0 /mnt/data/photos,mp=/data/photos/




JELLYFIN:



mkdir /data
mkdir /data/stream
mkdir /data/usb

pct set 106 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 106 -mp1 /mnt/data/usb/,mp=/data/usb





PLEX:

mkdir /data
mkdir /data/stream
mkdir /data/usb




pct set 108 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 108 -mp1 /mnt/pve/disk4tb/movies/,mp=/data/usb/Movies

pct set 108 -mp1 /mnt/data/usb/,mp=/data/usb


pct set 108 -mp1 /mnt/pve/disk4tb/movies/,mp=/data/usb/movies



Web:   plex.tv/claim

curl -X POST 'http://IP-Address:32400/myplex/claim?token=claim-XXXXXXXXXXXX'

systemctl restart plexmediaserver.service 


curl -X POST -s -H "X-Plex-Client-Identifier: {XXXXXXXXX}" "https://plex.tv/api/claim/exchange?token={claim-xxxxxxxxxx}"



RADARR:


apt install resolvconf


mkdir /data
mkdir /data/stream
mkdir /data/usb
mkdir /data/downloads
mkdir /data/
mkdir /data/radarr
mkdir /data/sonarr

mkdir /data/radarr
mkdir /data/radarr


nano /etc/pve/lxc/110.conf

pct set 109 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 109 -mp1 /mnt/data/usb/,mp=/data/usb
pct set 109 -mp1 /mnt/dve/disk4tb/movies,mp=/data/usb
pct set 109 -mp2 /mnt/pve/disk4tb/downloads,mp=/data/downloads


pct set 108 -mp0 /mnt/data/stream/,mp=/data/stream



      - /data/radarr/:/config
      - /data:/movies #optional
      - /data/downloads:/downloads #optional





