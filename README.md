# NETWORKWALKS-MARYAM-B084D-WK1-PM1-CYBERSECURITY-LAB-SETUP
A cyberSecurity Lab Setup
# My Kali Linux Lab on VirtualBox

Week 1 project from the NetworkWalks cybersecurity course. I built a small virtual lab on my Windows PC where Kali Linux sits on its own private network and still reaches the internet.

## What I built

A Kali Linux 2026.3 virtual machine running in VirtualBox 7.2.22. It lives on a custom NAT Network (10.0.0.0/24) and has a fixed IP of 10.0.0.2. Clipboard and drag and drop work both ways, and my Windows Downloads folder is shared into Kali. I took a snapshot at the end so I can always roll back to a clean setup.

## Network layout

 Item - Value 
 Network name - NatNetwork 
 Subnet - 10.0.0.0/24 
 Kali IP - 10.0.0.2 
 Gateway - 10.0.0.1 
 DNS | 8.8.8.8 

Later I plan to add Windows and Android machines on the same subnet as practice targets.

## How I set it up

1. **Installed VirtualBox.** The installer failed the first time, so I reran it as administrator.
2. **Got Kali.** I downloaded the pre-built VirtualBox image from kali.org and extracted it with 7-Zip.
3. **Made the network.** In VirtualBox (Ctrl+H), I created a NAT Network called NatNetwork with the prefix 10.0.0.0/24 and DHCP turned on.
4. **Imported the VM.** Machine, then Add, then picked the .vbox file.
5. **Changed the VM settings.** Adapter 1 on NatNetwork with Promiscuous Mode set to Allow All, clipboard and drag and drop on Bidirectional, my Downloads folder shared with auto mount and full access, and RAM raised to 4096 MB.
6. **Set the IP inside Kali.** Edit Connections, then Wired connection 1, then IPv4 set to Manual with 10.0.0.2, netmask 24, gateway 10.0.0.1 and DNS 8.8.8.8.
7. **Fixed the internet delay.** On newer Kali and VirtualBox 7, the connection can stall while Linux checks for duplicate addresses. I turned that check off:

```bash
sudo nmcli connection modify Wired\ connection\ 1 ipv4.dad-timeout 0
sudo nmcli connection down Wired\ connection\ 1
sudo nmcli connection up Wired\ connection\ 1
```

8. **Took a snapshot** called Fresh Kali Lab Setup after a clean shutdown.

## Proof it works

`ip a` shows eth0 on 10.0.0.2/24.

`ping -c 3 google.com` returned 3 replies with 0% packet loss.

The sf_Downloads folder shows up on the Kali desktop, and text copied in Windows pastes into Kali.

IP address/Ping test](screenshot-ip-ping.png)

## Problems I ran into

- Installer ended early.I restarted my PC and ran it again
- Defender deleted the Kali download. It flagged it as a Trojan, which is a known false positive for Kali because of the security tools inside. I confirmed the official source, excluded my C:\Kali folder and downloaded again.
- Ping gave invalid argument. I forgot the number after -c.

## What I learned

How a NAT Network keeps VMs on a private subnet while still giving them internet, how to set and check a static IP in Kali, why antivirus tools flag pentesting images, and how snapshots save me from breaking the lab.

## Next steps

- Add a Windows VM on this network and ping test between machines
- Add an Android VM
- Practice scanning and recon on machines I own

## Note

This lab is only for learning. Everything stays inside my own virtual network, and I only test machines I own or have permission to test.

Author: Princess ([Eisha001](https://github.com/Eisha001))
