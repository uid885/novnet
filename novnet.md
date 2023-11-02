Author: Christo Deale <br>
Date:   2023-11-02

# The Novnet HomeLab Environment

## The hardware used
![rsz_1lenovo_legion_y540_17irh_ansicht4](https://github.com/uid885/novnet/assets/135722741/a3a099b6-a759-43ae-bed3-d1aa4e9695ed)

## The Lenovo Y540-17IRH Specifications
| Part     | Description                    |
|---------:|:-------------------------------:|
| Type     | Lenovo                         |
| Model    | Legion Y540-17IRH              |
| Processor| i7-9750HF, 6Cores, 12Threads   |
| Graphics | NVIDIA GeFore GTX 1660 Ti (6GB GDDR6 ) |
| Memory   | 2x 32GB 2666MHz DDR4 - TOTAL 64GB |
| Storage  | 2TB NVMe, 1TB SSD              |
| Ethernet | Gigabit Ethernet Connection    |
| Power    | 230W AC Adapter                |

## XCP-ng as HyperVisor
I had the Y540 and did not want to spend more money on a power hungry old server. I have decided to use [XCP-NG](https://xcp-ng.org/) as my hypervisor as i did not see many Homelab's running XCP-NG <br>

Downloaded the XCP-ng [ISO](https://mirrors.xcp-ng.org/isos/8.2/xcp-ng-8.2.1.iso?https=1)  **<<** here <br>

I found this awesome tutorial on how to get started, and managed to get all up in running in no time:<br>
[How to Install XCP-ng Hypervisor](https://ostechnix.com/install-xcp-ng/) <br>
[Getting Started with XCP-ng Management Console](https://ostechnix.com/xcp-ng-management-console/) <br>
[How to Install Xen Orchestra Appliance (XOA) in XCP-ng Server](https://ostechnix.com/install-xen-orchestra-appliance-xoa/) <br>

> XCP-ng *HyperVisor Layer* > Xen Orchestra *Xen Orchestra Applicance XOA* 

## Bootup & Interface
![xcp-ng01](https://github.com/uid885/novnet/assets/135722741/65988c26-871f-4142-b4a3-b705affcba95)
*XCP-ng during boot process*

![xcp-ng02](https://github.com/uid885/novnet/assets/135722741/04f34026-bd71-4291-bf2d-3d14dd23b060)
*XCP-ng idle state*

## Interface idle state observations:
- Status Display (Have a quick glance of your server IP)
- Virtual Machines (Can view, start & stop individual VMs)
- Local Command Shell (Quick access to server using XE Console)

## XCP-ng Guest tools: 
The Guest Tools are a Management Agent which you need to install on your VMs. (Think simular to vmware tools) <br>
In my case the majority of the VMs are Red Hat RHEL9.2 boxes. For RHEL i had to follow the following steps:

### Step 1 - Enable EPEL 9
```
subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
```
### Step 2 - Install & enable & Start XE-GUEST Tools
```
dnf install xe-guest-utilities-latest
systemctl enable xe-linux-distribution
systemctl start xe-linux-distribution
```
### Step 3 - Windows XCP-ng Tools
On my Windows 10 & Windows Server 2022 boxes i had to mount the XCP-ng Tools drive (its a storage SR). <br>
Simply navigate to the "dvd rom" labled XCP-ng Tools, and install the setup.exe 

## Access to VMs
On my daily (Lenovo T460p), where i installed the XOA (Orchestra), I navigated to the static IP address of <br>
the server and logged into XOA <br>

![Screenshot from 2023-11-01 22-47-25](https://github.com/uid885/novnet/assets/135722741/c83fce1d-c0c0-4556-ac15-37a9c778c9f9)
*XOA Dashboard*

![Screenshot from 2023-11-01 22-46-56](https://github.com/uid885/novnet/assets/135722741/7fb61dac-9f16-4f3c-83b4-1cd261b861d5)
*VM overview*

![Screenshot from 2023-11-01 22-53-43](https://github.com/uid885/novnet/assets/135722741/3a985f85-74f4-4485-a666-1c33d9cd1c90)
*In Browser Access to VM's etc*

![Screenshot from 2023-11-02 15-06-36](https://github.com/uid885/novnet/assets/135722741/591b2e48-e525-4e95-bb40-6551b411a3d7)
*Accessing the XCP-ng server using XE-Console*

You can basically manage everything on your XCP-ng server from XOA. I love how simplistic and easy to ues the interface is.

## Another way I use to access the Server & VMs
I installed a free copy of [Remote Desktop Manager](https://devolutions.net/remote-desktop-manager/). <br>

![Screenshot from 2023-11-02 18-53-50](https://github.com/uid885/novnet/assets/135722741/70405601-cf55-405e-8a0f-abd95725eec0)
*Remote Desktop Manager, allowing multiple open tabs with different sessions to VMs (ssh, cockpit, xrpd, rdm etc)*

## Novnet Network Diagram
I used [online.visual-paradigm](https://online.visual-paradigm.com/) to create my diagram.
Consider novnet.co.za is a homelab use domain i created. 

![xcp_novnet2](https://github.com/uid885/novnet/assets/135722741/67b3959e-cf46-4fa9-a04a-2f8ee664e645)

As you can see I have several VMs on the HyperVisor, each service its own purpose.<br>
A Homelab is never done, and will change continuesly. Next up is to install [Tailscale](https://tailscale.com/) <br>
on my XCP-ng server so that i can access the lab from anywehere. Found instructions for <br>
[Tailscale on XCP-ng](https://techoverflow.net/2022/05/11/how-to-install-tailscale-on-xcp-ng-host/) here.
