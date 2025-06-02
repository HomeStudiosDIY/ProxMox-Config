# Work in Progress


# ProxMox-Config  <a id="readme-top"></a>

Here with some config settings to help you get you ProxMox Server setup and working!!  

I run all my application on a LXC inside Docker but you can run the LXC application directly this was just my preference for consistency as not all application I use can run directly on a LXC. All my LXC config and Docker Compose files will also be sheared.

https://community-scripts.github.io/ProxmoxVE/



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
		<a href="#nas-to-nfs">Connect to your NAS with NFS</a>
		  <ul>
			<li><a href="#built-with">Built With</a></li>
		  </ul>
    </li>
    <li>
		<a href="#install-nvidia-drivers-on-proxmox">Install NVIDIA Drivers</a>
		  <ul>
			<li><a href="#prerequisites">On Proxmox</a></li>
			<li><a href="#installation">ON LXC's</a></li>
		  </ul>
    </li>
    <li><a href="#usage">OpnSense</a></li>
    <li><a href="#roadmap">UniFi</a></li>
    <li><a href="#contributing">Vaultwarden</a></li>
    <li><a href="#license">Home Assistant</a></li>
    <li><a href="#contact">JellyFin</a></li>
    <li><a href="#acknowledgments">Plex</a></li>
	<li><a href="#acknowledgments">Frigate</a></li>
	<li><a href="#acknowledgments">Immich</a></li>
	<li>
		<a href="#acknowledgments">Media</a>
		  <ul>
			<li><a href="#prerequisites">Ombi</a></li>
			<li><a href="#installation">ON LXC's</a></li>
		  </ul>	
	</li>
	<li>
		<a href="#acknowledgments">Downloaders</a>
		  <ul>
			<li><a href="#prerequisites">ARR</a></li>
			<li><a href="#installation">Radarr</a></li>
		  </ul>	  
	</li>
  </ol>
</details>






Proxmox installer
nomodeset



<a id="nas-to-nfs"></a>
## Connect to your NAS with NFS

To connect to a NAS device with NFS you will have to setup some paths/directory’s this is how I have done mine but you can use your own location.   

If you need sub folders you will need to make the directory tree.

mkdir /mnt/data  
mkdir /mnt/data/stream  
mkdir /mnt/data/usb  
mkdir /mnt/data/photos  


mkdir /mnt/pve/disk4tb/frigate



mkdir /mnt/pve/disk4tb/downloads



The following will be needed to auto connect to you NFS shears.
 
nano /etc/fstab

10.0.0.1:/volume1/Stream/ /mnt/data/stream nfs defaults 0 0  
10.0.0.1:/volumeUSB1/usbshare /mnt/data/usb nfs defaults 0 0  
10.0.0.1:/volume1/Photos-Link /mnt/data/photos nfs defaults 0 0  
10.0.0.1:/volume1/Downloads /mnt/data/downloads nfs defaults 0 0  


Once you have saved your config you need to run the following.

Reload systemd: systemctl daemon-reload  
Mount shares: mount -a



chown 100109:100117 /mnt/pve/disk4tb/frigate/


<p align="right">(<a href="#readme-top">back to top</a>)</p>


## Install NVIDIA Drivers on ProxMox
<a id="install-nvidia-drivers-on-proxmox"></a>


### NVIDIA
<a id="install-nvidia-drivers-on-proxmox"></a>


apt update && apt upgrade -y && apt install pve-headers build-essential software-properties-common make nvtop htop -y
update-initramfs -u



wget https://uk.download.nvidia.com/XFree86/Linux-x86_64/550.142/NVIDIA-Linux-x86_64-550.142.run


chmod +x NVIDIA-Linux-x86_64-550.144.03.run


./NVIDIA-Linux-x86_64-550.144.03.run --dkms







### LXC Setup for Nvida: 
<a id="install-nvidia-drivers-on-proxmox"></a>



I have the following LXC setup to use my NVIDA card (Jellyfin, Plex, ......)





pct push 105 NVIDIA-Linux-x86_64-550.144.03.run /root/NVIDIA-Linux-x86_64-550.144.03.run

chmod +x NVIDIA-Linux-x86_64-550.144.03.run

./NVIDIA-Linux-x86_64-550.144.03.run --no-kernel-modules

apt install gpg curl

curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | tee /etc/apt/sources.list.d/nvidia-container-toolkit.list


apt update  
apt install nvidia-container-toolkit  
nvidia-smi  

only if you run Docker  
nvidia-ctk runtime configure --runtime=docker




nano /etc/nvidia-container-runtime/config.toml  
#no-cgroups = false  
to  
no-cgroups = true  



ls -al /dev/nvidia*


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





<p align="right">(<a href="#readme-top">back to top</a>)</p>





## Opensence
<a id="about-the-project"></a>


bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/vm/opnsense-vm.sh)"





<p align="right">(<a href="#readme-top">back to top</a>)</p>

## UniFi
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"
	

<a href="https://github.com/HomeStudiosDIY/ProxMox-Config/blob/main/Docker%20Compose%20Files/Unifi/init-mongo.sh" target="_blank">file</a>


<a href="https://github.com/HomeStudiosDIY/ProxMox-Config/blob/main/Docker%20Compose%20Files/Unifi/Unifi.yaml" target="_blank">file Docker</a>



<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Vaultwarden
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"



<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Home Assistant
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/vm/haos-vm.sh)"


<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Jellefin Setup and Config
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"


JELLYFIN:



mkdir /data
mkdir /data/stream
mkdir /data/usb

pct set 106 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 106 -mp1 /mnt/data/usb/,mp=/data/usb

RamDisk:
lxc.mount.entry: tmpfs dev/shm tmpfs size=4G,nosuid,nodev,noexec,create=dir 0 0




<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Plex Setup and Config
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"
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




<p align="right">(<a href="#readme-top">back to top</a>)</p>

## FRIGATE:
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"


Privileged


 To update Frigate, create a new container and transfer your configuration.


mkdir /data/
mkdir /data/config/
mkdir /data/cctv/


pct set 104 -mp0 /mnt/pve/disk4tb/frigate,mp=/data/cctv/




nano /etc/pve/lxc/104.conf

lxc.mount.entry: /dev/bus/usb/002/ dev/bus/usb/002/ none bind,optional,create=dir 0,0






<p align="right">(<a href="#readme-top">back to top</a>)</p>

## IMMICH:
<a id="about-the-project"></a>

bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/docker.sh)"

immich

mkdir /data
mkdir /data/immich
mkdir /data/photos

pct set 106 -mp0 /mnt/data/photos,mp=/data/photos/









<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Radar:
<a id="about-the-project"></a>




mkdir /data
mkdir /data/stream
mkdir /data/usb
mkdir /data/downloads
mkdir /data/radarr
mkdir /data/sonarr




nano /etc/pve/lxc/110.conf

pct set 109 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 109 -mp1 /mnt/data/usb/,mp=/data/usb
pct set 109 -mp1 /mnt/dve/disk4tb/movies,mp=/data/usb
pct set 109 -mp2 /mnt/pve/disk4tb/downloads,mp=/data/downloads


pct set 108 -mp0 /mnt/data/stream/,mp=/data/stream



      - /data/radarr/:/config
      - /data:/movies #optional
      - /data/downloads:/downloads #optional


<p align="right">(<a href="#readme-top">back to top</a>)</p>

##  Sonarr:
<a id="about-the-project"></a>

mkdir /data
mkdir /data/stream
mkdir /data/usb
mkdir /data/downloads
mkdir /data/sonarr




nano /etc/pve/lxc/110.conf

pct set 109 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 109 -mp1 /mnt/data/usb/,mp=/data/usb
pct set 109 -mp1 /mnt/dve/disk4tb/movies,mp=/data/usb
pct set 109 -mp2 /mnt/pve/disk4tb/downloads,mp=/data/downloads


pct set 108 -mp0 /mnt/data/stream/,mp=/data/stream

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Downloader
<a id="about-the-project"></a>

mkdir /data
mkdir /data/stream
mkdir /data/usb
mkdir /data/downloads
mkdir /data/
mkdir /data/radarr
mkdir /data/sonarr




nano /etc/pve/lxc/110.conf

pct set 109 -mp0 /mnt/data/stream/,mp=/data/stream
pct set 109 -mp1 /mnt/data/usb/,mp=/data/usb
pct set 109 -mp1 /mnt/dve/disk4tb/movies,mp=/data/usb
pct set 109 -mp2 /mnt/pve/disk4tb/downloads,mp=/data/downloads


pct set 108 -mp0 /mnt/data/stream/,mp=/data/stream


apt install resolvconf

<p align="right">(<a href="#readme-top">back to top</a>)</p>