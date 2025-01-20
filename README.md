# Project PiMox
Setting up Proxmox VE on Raspberry Pi

This is made in an attempt to document my process in setting up my own Proxmox server using a Raspberry Pi (RPi). I have been planning for so long in self hosting the services that I use. My initial plan was to make a Pi NAS but upon reading the performance of RPi 4B as a NAS, I decided to postpone that plan and push it for later.

First of all why in an RPi? Well, this is the only spare "computer" that I have lying around collecting dust. This board was scrapped out from our Design Project back in my last year in college. Also, I believe that this is a smart move considering that this is my first attempt in building my own server, I will be saving as much as I can to prevent over-commitment.

At first, I started out hosting Pi-hole in tandem with WireGuard via PiVPN. Upon reading about self hosting and homelabbing, I stumbled accross a software hypervisor named Proxmox. I learned that even though it is not supported by Proxmox itself, there are maintained ARM64 ports that will enable me to install the service on RPi aka PiMox.