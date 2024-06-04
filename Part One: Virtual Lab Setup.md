# Part One: Virtual Lab Setup

### Blog Post: https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part

Note: Follow steps AFTER downloading VMware Workstation as well as the VMs required. Relevant links from the blog will be provided below:

Creating a new VM in Workstation: https://kb.vmware.com/s/article/1018415

Ubuntu Server Download: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-live-server-amd64.iso

Windows VM Download: https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/ 

## Setup

### Ubuntu Server 

In your VMWare Workstation selection, choose "Create a New Virtual Machine".

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7a92fdf7-b628-4742-af1e-f625be025c7d)

After doing so, select the following or the equivalent file downloaded from the Ubuntu Server Download: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/fd30a489-10bb-455d-bc3d-e67d0e85c7b3)

In the New Virtual Machine Wizard, create a name and choose a location, selecting "Next" when finished:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/69234001-6967-4695-8ac7-fbe3416fa257)

When reaching this portion following the blog, it is recommended that the "Hard Disk" is at least 14 GB (I chose to do 20 GB) and that 2 CPU cores with 2 GB RAM are dedicated to the server.

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/633bf19d-2005-47f0-9a17-d6fce822f79b)

Once "Finish" is selected, the Ubuntu server will take a moment to boot up, leading to this screen once you interact with it: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/03837488-965b-4db9-8241-9cb5b21d1766)

On the "Installer update available" screen, select "Continue without updating":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7e4606dd-4f7e-4f5c-8715-0865c60db73e)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/24696e4c-8aa9-48dc-b680-8491a57cc6f8)

Upon reaching the "Choose type of install" screen, you want to select the "Ubuntu Server" default installation:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a59bb674-f4f7-462e-899a-e13c0d533842)

