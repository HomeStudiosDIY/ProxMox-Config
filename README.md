# ProxMox-Config

Here is some settings and how to's to get your proxmox server setting done!!  


# Connect to your NAS with NFS 

nano /etc/fstab

10.0.0.1:/volume1/Stream/ /mnt/data/stream nfs defaults 0 0  
10.0.0.1:/volumeUSB1/usbshare /mnt/data/usb nfs defaults 0 0  
10.0.0.1:/volume1/Photos-Link /mnt/data/photos nfs defaults 0 0  
10.0.0.1:/volume1/Downloads /mnt/data/downloads nfs defaults 0 0  



Reload systemd: systemctl daemon-reload  
Mount shares: mount -a  


# NVIDIA Drivers Install
