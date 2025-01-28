# Project PiMox
Setting up Proxmox VE on Raspberry Pi

This is made in an attempt to document my process in setting up my own Proxmox server using a Raspberry Pi (RPi). I have been planning for so long in self hosting the services that I use. My initial plan was to make a Pi NAS but upon reading the performance of RPi 4B as a NAS, I decided to postpone that plan and push it for later.

First of all why in an RPi? Well, this is the only spare "computer" that I have lying around collecting dust. This board was scrapped out from our Design Project back in my last year in college. Also, I believe that this is a smart move considering that this is my first attempt in building my own server, I will be saving as much as I can to prevent over-commitment.

At first, I started out hosting Pi-hole in tandem with WireGuard via PiVPN. Upon reading about self hosting and homelabbing, I stumbled accross a software hypervisor named Proxmox. I learned that even though it is not supported by Proxmox itself, there are maintained ARM64 ports that will enable me to install the service on RPi aka PiMox.

## Hardware
- Raspberry Pi 4B (I have the 4GB version)
- USB C Power Supply
- 2.5 inch Sata to USB enclosure
- SATA SSD (Optional | You can use SD Card)
- External HDD (Optional | This is for my media storage)
- Pi case with fan
- Ethernet Cable
- Other computer to SSH from

## Preparation
In this tutorial, I used the official Raspberry Pi OS as the base operating system (OS) of my Rpi. I have seen other people use DietPi for a lighter OS. I chose Raspberry Pi OS Lite 64 bit since I will be running this headless and just SSH using my Macbook Air later on.

I used the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to flash the OS directly on my SATA SSD. After that, I first boot up the system then check if my date and time is synchronized. Run `timedatectl` and if you see that your RPi is not sychronized modify the NTP servers in `sudo nano /etc/systemd/timesyncd.conf`. In my case, since I am in the Philippines, I used:
```bash
NTP=ph.pool.ntp.org
NTP=0.asia.pool.ntp.org
NTP=1.asia.pool.ntp.org
NTP=2.asia.pool.ntp.org
NTP=3.asia.pool.ntp.org
```
Then restart the timesyncd.service `sudo systemctl restart systemd-timesyncd.service`. Wait for it to synchronize then perform the basic `sudo apt update && sudo apt upgrade`.

## Installation of Proxmox ARM64 port
I followed a this guide by [Emmet](https://pimylifeup.com/raspberry-pi-proxmox/) that gives a detailed installation guide for PiMox.

## Post installation
I recently discovered this [Proxmox Helper-scripts](https://pimox-scripts.com/) dedicated for ARM64 chips. This is REALLY REALLY helpful since the original Proxmox Helper-scrips are not going to work in our PiMox host. I wanted to share this website because I haven't seen any guide mentioning this.

I first run the Proxmox Post-install script to prepare my node. This will disable unnecessary things such as the subscription nag and add proper repositories when updating your host.
```bash
# Proxmox VE Post Install
bash -c "$(wget -qLO - https://github.com/asylumexp/Proxmox/raw/main/misc/post-pve-install.sh)"
```

After that, you are done! You can finds lots of services you can run in the Pimox Helper-scripts website. 

### Things I can recommend running though are: 
**Wireguard** for running your own VPN server
**Pi-hole with Unbound** for running your own recursive ad-blocking DNS server
