# Project PiMox

Setting up Proxmox VE on Raspberry Pi

This is made in an attempt to document my process in setting up my own Proxmox server using a Raspberry Pi (RPi). I have been planning for so long in self hosting the services that I use. My initial plan was to make a Pi NAS but upon reading the performance of RPi 4B as a NAS, I decided to postpone that plan and push it for later.

First of all why in an RPi? Well, this is the only spare "computer" that I have lying around collecting dust. This board was scrapped out from our Design Project back in my last year in college. Also, I believe that this is a smart move considering that this is my first attempt in building my own server, I will be saving as much as I can to prevent over-commitment.

At first, I started out hosting Pi-hole in tandem with WireGuard via PiVPN. Upon reading about self hosting and homelabbing, I stumbled accross a software hypervisor named Proxmox. I learned that even though it is not supported by Proxmox itself, there are maintained ARM64 ports that will enable me to install the service on RPi aka PiMox.

## Hardware

- Raspberry Pi 4B (I have the 4GB version)
- USB C Power Supply
- 2.5 inch Sata to USB enclosure
- SATA SSD (or you can use SD Card)
- Pi case with fan (I recommend using a fan to prevent your RPi from thermal throttling)
- Ethernet Cable
- Other computer to SSH from

### Optional

- Keyboard
- Mouse
- Monitor
- External HDD (This is for my media storage)

## Preparation

In this guide, I used the official Raspberry Pi OS as the base operating system (OS) of my Rpi.

> I have seen other people use DietPi for a lighter OS.

I used the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to flash the OS directly on my SATA SSD. Download the RPi Imager tool then choose your device, in my case Raspberry Pi 4. Select your OS, I will be using PiOS Lite 64bit. Then lastly select the media storage that you want your RPi to boot up from, mine is the SATA SSD I mentioned earlier.

(images/pi-imager.png)

Before proceeding with the installation, I recommend that you also setup your SSH configuration here, you will be using the SSH credentials to access your RPi from your other PC via SSH. You may also disable WLAN connection since it is recommended to do this in a wired connection (I haven't tried setting up Proxmox using WiFi).

After that, boot up the system and connect to it via SSH from your computer (You can also plugin a keyboard, mouse, and monitor if that's what you prefer). I use Termius on my Mac, you may also use the native terminal if you are in MacOS.

> I just prefer the feels of Termius.

If you are using Windows, you may install PuTTY then SSH from there.

You can then check if my date and time is synchronized.

> I don't know if I am the only one encountering this but my Pi always have unsynchronized Time and Date.

Run `timedatectl` and if you see that your RPi is not sychronized modify the NTP servers in `sudo nano /etc/systemd/timesyncd.conf`. In my case, since I am in the Philippines, I used:

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

Let us first install curl:  
`sudo apt install curl`

It is recommended to set a static IP address on your RPi. To do this we can edit it via:  
`sudo nano /etc/hosts`

You will find the host name that you set and an IP beside it:  
`127.0.0.1` change this to whatever IP that is available in your network.

After you input the IP address, you may now save it by:  
`CTRL` + `x` then `Y` then `ENTER`

You can check if the changes were made by running:  
`hostname -i`

You must now set a root password as this wil be the one you are using when accessing the Proxmox WebUI:  
`sudo passwd root`

## Post installation

I recently discovered this [Proxmox Helper-scripts](https://pimox-scripts.com/) dedicated for ARM64 chips. This is REALLY REALLY helpful since the original Proxmox Helper-scrips are not going to work in our PiMox host. I wanted to share this website because I haven't seen any guide mentioning this.

I first run the Proxmox Post-install script to prepare my node. This will disable unnecessary things such as the subscription nag and add proper repositories when updating your host.

```bash
# Proxmox VE Post Install
bash -c "$(wget -qLO - https://github.com/asylumexp/Proxmox/raw/main/misc/post-pve-install.sh)"
```

After that, you are done! You can finds lots of services you can run in the Pimox Helper-scripts website.

### Things I can recommend running though are:

- **Wireguard** for running your own VPN server
- **Pi-hole with Unbound** for running your own recursive ad-blocking DNS server
