# Home Lab Setup: Kali Linux + Ubuntu Server on VirtualBox

## **Objecttive**
- Build a safe and isolated place to study and practice cyversecurity
- Create 2 virtual machines (VMs) that can communicate internally via Host-Only network.

## **Tools and Environment**
- Oracle VirtualBox
- Kali Linux 2025. 1a (Installer ISO)
- Ubuntu Server 24.04.2 LTS (Installer ISO)
- Host Machine: Ubuntu Desktop (or any OS)

## **Lap SetUp Steps**

### **1. Download VirtualBox**
- Official website:
https://www.virtualbox.org/wiki/Downloads
- Choose version matches operating system
- Install it using provided installer

### **2. Kali Linux Installation**
#### **Create a Virtual Machine**
- Official Website:
https://www.kali.org/get-kali/#kali-installer-images
- Name: Kali-Linux
- OS Type: Linux --> Ubuntu (64-bit)
- RAM: 2048MB
- Disk: 30GB VDI (Dynamically allocated)

#### **Network Configuration**
- Adapter 1: Host-only Adaper (Name: vboxnet0)

#### **Installation Highlights**
- Choose Graphical Install when booting from ISO
- Language: English
- Keyboard: Americian English
- Set hostname, username and password
- Skip network configuration if DHCP is unavailable during install
- Finish installation and remove the ISO before rebooting

#### **Static IP Addressing (after installation)**
sudo ip addr add 192.168.56.10/24
dev eth0
sudo ip link set eth0 up

Or configure permanently in etc/network/interfaces:
auto eth0
iface eth0 inet static
    address 192.168.56.10
    netmask 255.255.255.0


### **3. Ubuntu Server Installation**
#### **Create a Virtual Machine**
- Official Website:
https://ubuntu.com/download/desktop
- Name: Ubuntu-Server
- OS Type: Linux --> Ubuntu (64-bit)
- RAM: 2048MB
- Disk: 20GB VDI (Dynamically allocated)

#### **Network Configuration**
- Adapter 1: Host-only Adaper (Name: vboxnet0)

#### **Installation Highlights**
- Select Install Ubuntu Server at boot
- Language: English
- Keyboard: Americian English
- Manual network configuration:
    - Subnet: 192.168.56.0/24
    - Address: 192.168.56.20
    - Gateway: leave blank
    - Name server: 8.8.8.8
- Skip proxy settings
- Use default Ubuntu mirror
- Paritioning:
    - Use an entire disk
    - Do not use LVM or disk encryption
- Create user and password
- Install Open SSH Server (important for SSH access)
- Allow password authentication for SSH
- Skip additional Snap packages.

#### **After installation**
- Remove installation ISO
- Reboot
- Login using username and password


## **Lap Network Overview**
  +-------------------------------------------------------------+
|                      VirtualBox Host-Only Network           |
|                          (192.168.56.0/24)                  |
|                                                             |
|   +--------------------------------+    +--------------------------------+ 
|   | Kali Linux                     |    | Ubuntu Server                  | 
|   | IP: 192.168.56.10               |    | IP: 192.168.56.20               | 
|   | Role: Attacker / Testing Tool   |    | Role: Target / Vulnerable Host  | 
|   +--------------------------------+    +--------------------------------+ 
|                                                             |
+-------------------------------------------------------------+

## **Connectivity Check**
From Kali-Linux:
ping 192.168.56.20

From Ubuntu-Server:
ping 192.168.56.10

Result: Both VMs should successfully communicate internally.

## **Notes**
- Host-only Network isolates the lab environment from the Internet
- No external Internet access is available for security
- Perfect for learning, practicing scanning, exploiting, and more...

-*By Phong 2025*-
