
Overview



Setup Requirements



Setup Guides




<details>
  <summary><u>Table of Contents</u></summary>
  <ol>
    <li>
		<a href="#nas_to_nfs">Connect to your NAS with NFS</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>
    </li>
    <li>
		<a href="#adding_nvidia_drivers">Adding NVIDIA Drivers to Proxmox</a>
		  <ul>
			<li>
				<a href="#prerequisites">On Proxmox</a>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>	
			</li>
			<li>
				<a href="#installation">ON LXC's</a>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>					
			</li>
		  </ul>
    </li>
    <li>
		<a href="#nas-to-nfs">Connect to your NAS with NFS</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>
    </li>
    <li>
		<a href="#install-nvidia-drivers-on-proxmox">Install NVIDIA Drivers</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>	
    </li>		
    <li>
		<a href="#usage">OpnSense</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>
	</li>
    <li>
		<a href="#roadmap">UniFi</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>		
	</li>
    <li>
		<a href="#contributing">Vaultwarden</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
    <li>
		<a href="#license">Home Assistant</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
    <li>
		<a href="#contact">JellyFin</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
    <li>
		<a href="#acknowledgments">Plex</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
	<li>
		<a href="#acknowledgments">Frigate</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
	<li>
		<a href="#acknowledgments">Immich</a>
			<ul>
				<li><a href="#built-with">Overview</a></li>
				<li><a href="#built-with">Setup Requirements</a></li>
				<li><a href="#built-with">Setup Guide</a></li>
			</ul>			
	</li>
	<li>
		<a href="#acknowledgments">Media</a>
		  <ul>
			<li>
				<a href="#prerequisites">Ombi</a>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>
			</li>
			<li>
				<a href="#installation">ON LXC's</a>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>					
			</li>
		  </ul>	
	</li>
	<li>
		<a href="#acknowledgments">Downloaders</a>
		  <ul>
			<li>
				<a href="#prerequisites">ARR</a></li>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>				  
			<li>
				<a href="#installation">Radarr</a>
					<ul>
						<li><a href="#built-with">Overview</a></li>
						<li><a href="#built-with">Setup Requirements</a></li>
						<li><a href="#built-with">Setup Guide</a></li>
					</ul>				
			</li>
		  </ul>	  
	</li>
  </ol>
</details>






## Connect to your NAS with NFS


### <u>Overview</u>


To connect to a NAS device with NFS you will have to setup some paths/directory’s this is how I have done mine but you can use your own location.   

### <u>Setup Requirements</U>

If you need sub folders you will need to make the directory tree.
	
mkdir /mnt/data  
mkdir /mnt/data/stream  
mkdir /mnt/data/usb  
mkdir /mnt/data/photos  


mkdir /mnt/pve/disk4tb/frigate



mkdir /mnt/pve/disk4tb/downloads


### <u>Setup Guide</u>

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






## Install NVIDIA Drivers on ProxMox
<a id="adding_nvidia_drivers"></a>

### <i>Overview</i>

### <i>Setup Requirements</i>

make sure the drives are the same on the host and LXC

### <i>Setup Guide</i>


1. #### <p><u>NVIDIA:</u>
<a id="install-nvidia-drivers-on-proxmox"></a>



<p> &nbsp; &nbsp; &nbsp; &nbsp; apt update && apt upgrade -y && apt install pve-headers build-essential software-properties-common make nvtop htop -y</p>

&nbsp; &nbsp; &nbsp; &nbsp; update-initramfs -u



>>wget https://uk.download.nvidia.com/XFree86/Linux-x86_64/550.142/NVIDIA-Linux-x86_64-550.142.run


chmod +x NVIDIA-Linux-x86_64-550.144.03.run


./NVIDIA-Linux-x86_64-550.144.03.run --dkms

</P>





2. #### <u>LXC Setup for Nvida:</u>
<a id="install-nvidia-drivers-on-proxmox"></a>


Overview

Setup Requirements

Setup Guide






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

