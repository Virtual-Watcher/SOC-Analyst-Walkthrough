# Part One: Virtual Lab Setup

### Blog Post: https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part

Note: Follow steps AFTER downloading VMware Workstation as well as the VMs required. Relevant links from the blog will be provided below:

Creating a new VM in Workstation: https://kb.vmware.com/s/article/1018415

Ubuntu Server Download: https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-live-server-amd64.iso

Windows VM Download: https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/ 


## (Ubuntu) Linux Virtual Machine Server Setup

### Initial Setup

In your VMWare Workstation selection, choose "Create a New Virtual Machine".

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7a92fdf7-b628-4742-af1e-f625be025c7d)

After doing so, select the following or the equivalent file downloaded from the Ubuntu Server Download: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/fd30a489-10bb-455d-bc3d-e67d0e85c7b3)

In the New Virtual Machine Wizard, create a name and choose a location, selecting "Next" when finished:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/69234001-6967-4695-8ac7-fbe3416fa257)

When reaching this portion following the blog, it is recommended that the "Hard Disk" is at least 14 GB and that 2 CPU cores with 2 GB RAM are dedicated to the server.

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/32793ba2-2b87-4d90-8602-572f0063946d)

### Ubuntu Server Installation

Once "Finish" is selected, the Ubuntu Server will take a moment to boot up, leading to this screen once you interact with it: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/03837488-965b-4db9-8241-9cb5b21d1766)

On the "Installer update available" screen, select "Continue without updating":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7e4606dd-4f7e-4f5c-8715-0865c60db73e)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/24696e4c-8aa9-48dc-b680-8491a57cc6f8)

Upon reaching the "Choose type of install" screen, you want to select the "Ubuntu Server" default installation:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a59bb674-f4f7-462e-899a-e13c0d533842)

When progressing to the portion of the installation shown below, I took an alternate route to continue. In the blog, Eric suggests that you should set a static IP address for the VM. However, I was able to leave my own VM as is throughout the walkthrough and had no issues with the IP changing. You can choose to follow the blog and the related steps in setting a static IP or you can choose to leave the settings as the default, though I recommend that you write down the "DHCPv4" field for your Ubuntu Server.

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/af4553ff-02fb-46f3-8b3a-031f00eb0932)

Some storage configuration settings, to reflect what I kept during the walkthrough:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1572a77d-4d65-49ce-86f0-c85daf72b087)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/3b6785c1-12fc-42e0-b7e9-6acaec7d1f53)

You will want to select "Continue" on this screen:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/76f25e43-848d-4466-9859-9335dac74f46)

Once you reach the profile setup, you can choose any name, server name, username, and password. Be sure to remember or write down this information as you will need it. I decided to follow along with the blog walkthrough and set the same credentials as the purpose of the service is only for practicing exercises in a guided lab environment. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f3b20f64-424a-4bde-b4a0-0094cdb4ab56)

On the following screen, you want to select "Install OpehSSH Server" and make sure the "X" is visible within the brackets. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/05bc8e3d-82d6-4d3d-91e3-26e17f2b55f9)

After which, you will be met with this installation screen and will want to wait until the "Install complete!" appears in the top left corner and the "Reboot Now" option is visible below:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/54748b7a-4cd8-4db5-a074-8a9d84cc1c32)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/682b19b9-941a-4dae-b2e2-a317248b113f)

### Server Login and Test

When the Ubuntu Server has finished rebooting, you will eventually be met with the "server name" login field as well as the password field, enter what you created previously:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/ec167f8f-df60-4f1c-892d-c12b1eeb1757)

The next step after logging in is to perform a DNS ping test, with the following command:
```
ping -c 2 google.com
```
Which should result in: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/028d38bf-4efa-41e0-b25a-c5183f64f3f5)

If your Ubuntu Server returns a similar output, you are ready to proceed in setting up your Windows Virtual Machine! 

## Windows Virtual Machine Setup

### Initial Setup

Within the VMWare Workstation selection screen, we will want to select "Open a Virtual Machine": 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/deac86f7-24fa-4230-ac15-1d3b542c6727)

After which, we will want to select our respective Windows Virtual Machine:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/feb6b25b-3cad-4813-88d7-6d1675dca172)

Then we will want to create a name and choose the location where the VM will be stored: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/31ed466b-b401-4991-84e1-9a63466f1959)

### Disabling Windows Defender

Once the Windows VM is imported and we are logged in, we will want to disable Windows Defender. 
