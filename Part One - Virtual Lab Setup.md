# Part One - Virtual Lab Setup

### Blog Post: [So you want to be a SOC Analyst? Part 1](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part)

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

Once the Windows VM is imported and we are logged in, we will want to disable Windows Defender. As stated in the blog, we want to disable Defender so it does not interfere with the exercises we have planned as Defender will detect and restrict our actions. Following along with the walkthrough, we take the following actions: 

Click the Windows icon next to the search bar and then click "Settings" underneath "Pinned".

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/5dc9c279-024a-4606-9b45-ac36fed4f4d5)

Select "Windows Security" underneath Privacy & security:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4ed54ca1-ee29-43c3-a96b-d9007aa3bc89)

Then choose "Virus & threat protection":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f456bd9d-5308-4503-9891-6dbaed22e12b)

Click "Manage settings" underneath Virus & threat protection settings:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/8bec99a8-253a-4b00-bd13-4fb648634c0f)

Turn "Tamper Protection" off:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/720af128-d900-40f4-81db-f14147704304)

To ensure that other settings do not interfere with our exercises, turn off the rest of the options as well:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/66cf3079-dd51-4406-b93a-33506be9fa78)

Open a command prompt as administrator by typing "cmd" in the search bar and then right-clicking the "Command Prompt" and then selecting "Run as administrator":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a8f4ac78-0d1b-4ef0-8279-20957205d2ce)

Run the following command:
```
gpedit.msc
```

This should bring up the Local Group Policy Editor, which we will need to navigate through and change certain policy settings and registries. First we will need to follow this path:
```
Computer Configuration > Administrative Templates > Windows Components > Microsoft Defender Antivirus
```

Then click on the "Microsoft Defender Antivirus" folder:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7150ad37-0ee5-4912-9610-80b22b74be12)

Double-click "Turn off Microsoft Defender Antivirus":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/935666cd-b286-475c-8f36-4879ad679e56)

Select "Enabled":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e04152b4-f96d-4a78-b27a-232ef74d9ff9)

Select "Apply" and then "OK": 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7d48adf2-b132-4dca-8e55-0cf78e866aa8)

In the previous command prompt, copy and paste this command:
```
REG ADD "hklm\software\policies\microsoft\windows defender" /v DisableAntiSpyware /t REG_DWORD /d 1 /f
```

If successful, you should see something similar: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/9830cc7f-04a2-4731-84e4-fa262bdb4e03)

Prepare to boot into Safe Mode to disable all Defender Services. Type "msconfig" into the search bar within the Start Menu and then open "System Configuration":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/43848bb7-699d-456d-9f0d-e4c555ec89b0)

Under the "Boot" tab, check "Safe boot" and then click "Apply" and "OK". This will begin to restart the computer in Safe Mode:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/23796886-90f8-403c-86cd-1332109df190)

Click "Restart":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/6085e8a0-b823-4f60-a176-cba93d0fc12b)

While in Safe Mode, go to the Start search bar and type "regedit", opening "Registry Editor":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7e6a4c07-552e-4879-b006-51e6796317ce)

Once opened, we will need to go to the following key locations, finding the "Start" value and changing it to "4":
```
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdBoot`
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdFilter`
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisDrv`
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisSvc`
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinDefend`
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Sense`
```

The "Start" value for each of these should look like this: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f2f933f8-ac3d-4f59-b0eb-c78070b6da48)

After clicking it, you will be asked to edit the value. Change the "Value data" to "4":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a1a4d0f2-8111-486f-a941-197c2ee0cca0)

Once we have changed the Start value to 4 for each of the key locations, we can proceed to leave Safe Mode. First, we repeat this previous step:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e2f7afc6-a593-4c00-b63e-d6a5efd7d7a9)

Secondly, we uncheck "Safe boot" and then select "Apply" and "OK":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/752bbb29-006c-4a19-8697-0baddec6a43e)

Lastly, we click "Restart":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/93851354-b049-4a18-9d36-ea9661014ad4)

Doing this should return us to our regular Desktop mode.

As recommended in the blog series, we will now adjust our settings to prevent the VM from going into standby as we perform our exercises. From an administrative command prompt, use these commands to prevent the VM from going to sleep and into standby:
```
powercfg /change standby-timeout-ac 0
powercfg /change standby-timeout-dc 0
powercfg /change monitor-timeout-ac 0
powercfg /change monitor-timeout-dc 0
powercfg /change hibernate-timeout-ac 0
powercfg /change hibernate-timeout-dc 0
```

With this done, we are now ready to install Sysmon.

### Installing Sysmon

Type "PowerShell" into the Start search bar and right-click "Windows Powershell", selecting "Run as administrator":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/17e92d9e-a5f6-4a91-88e6-47445ea14fa5)

Within the Administrative Windows PowerShell, you can download Sysmon with the following command:
```
Invoke-WebRequest -Uri https://download.sysinternals.com/files/Sysmon.zip -OutFile C:\Windows\Temp\Sysmon.zip
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/381aa405-7638-47a6-acb9-b82f1d745ca8)

Unzip Sysmon.zip:
```
Expand-Archive -LiteralPath C:\Windows\Temp\Sysmon.zip -DestinationPath C:\Windows\Temp\Sysmon
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/8854006e-d91f-4bc6-bb0b-ad3853e8a2fd)

Download [SwiftOnSecurity](https://infosec.exchange/@SwiftOnSecurity)’s Sysmon config:
```
Invoke-WebRequest -Uri https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml -OutFile C:\Windows\Temp\Sysmon\sysmonconfig.xml
```

Install Sysmon with Swift’s config:
```
C:\Windows\Temp\Sysmon\Sysmon64.exe -accepteula -i C:\Windows\Temp\Sysmon\sysmonconfig.xml
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f88713eb-446a-4fe1-acc3-c5877250b2b5)

Validate Sysmon64 service is installed and running:
```
Get-Service sysmon64
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e15fc181-c81a-4e23-8c49-9fca911a4dc2)

Check for the presence of Sysmon Event Logs:
```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a733e5db-892a-4144-9ebf-3ee6957c2f19)

## Installing [LimaCharlie](https://app.limacharlie.io/signup) 

Create a free account, fill in the questions asked to what is relevant for you

After, create an organization - as per the blog (since the blog did porkchop-sandwiches for the "Name", I decided to go with "mayonnaise-instrument"): 
```
From the blog: 
- Name: `whatever you want, but it must be unique`
    
- Data Residency: `whatever is closest`
    
- Demo Configuration Enabled: `disabled`
    
- Template: `Extended Detection & Response Standard`
```

For example:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7850d714-de8f-4639-9b95-eebe7b052882)

Then add a Sensor: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/8678850d-6006-4a94-8c49-7eac1e05ba3b)

Create an Installation Key:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/73c32bb0-0fbd-4dac-83f7-f68f3c95a2c3)

Select an Installation Key: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/dd4e78fb-157f-4804-a844-dbfb4f95adda)

Specify the x86-64 (.exe) but wait, as we need to do some commands on the Windows VM:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/fa798931-cbda-4263-8f2d-fde2590ae3b8)

In the Administrative PowerShell prompt, we will travel to this directory:
```
cd C:\Users\User\Downloads
```

Then use the following command:
```
Invoke-WebRequest -Uri https://downloads.limacharlie.io/sensor/windows/64 -Outfile C:\Users\User\Downloads\lc_sensor.exe
```

You will need to also shift to a standard command prompt with the following command:
```
cmd.exe
```

You will then need to copy the following command line argument based on your sensor: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a9a39fb2-9c25-4aa5-a9b4-763a0e7af269)

HOWEVER, you must type this into the command prompt, before pasting the command, with a space between this input and the copied argument for your sensor:
```
lc_sensor.exe
```

For example: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0a4c9dc5-8ae3-4bf8-a0a6-adcca9e33381)

If successful, the following should be the output: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/12e4b894-f7a3-440e-a7c1-aeace0d9d64f)

Likewise, on the sensor page where you copied the argument, the "Detected new sensor!" message should show: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a117636a-55a6-4b15-8db4-813db39aba29)

Returning the the LimaCharlie page, you will want to navigate the side tab, selecting "Artifact Collection" under "Sensors":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7640d328-a248-4bf2-8f98-d5cc39ca3685)

Then you will want to "Add Artifact Collection Rule":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/c0a79d6a-c581-4a0f-9799-622a06f9bbf4)

Then, following the blog, we will input the following:
```
1. Name: `windows-sysmon-logs`
    
2. Platforms: `Windows`
    
3. Path Pattern: `wel://Microsoft-Windows-Sysmon/Operational:*`
    
4. Retention Period: `10`
    
5. Click “Save Rule”
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/49d6d80a-41bc-4098-98dc-b4506c3717f2)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/c1aa1be6-8cd9-4b4e-a892-e21ac4bafb44)

With this step done, we are now ready to set up the attacker for these exercises: 

## Setting Up the Attacker
After logging in and having access to the command line, we can first do the following command to get the IP for our Ubuntu Linux Server:
```
sudo apt install net-tools
```

After, we can do the following command; which should give us our "ens33" IP which we will need for our next steps:  
```
ifconfig
```

We can start up another virtual machine and open up a terminal as advised in the walkthrough. I will be using Kali Linux out of personal preference, typing this into the command line:
```
ssh user@<ens33IP>
```

An example on my own terminal:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/98240f52-f7dd-46d0-81b9-79c10c0b0429)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/456e6924-351d-4855-b318-ecf51897c158)

After using SSH to connect to our Ubuntu Server, drop into a root shell with the following command:
```
sudo su
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/b0fd1bfa-4a2b-474a-91e1-9e1a8fdce31d)

Now, use the commands shown below, as provided from the walkthrough:
```
# Download Sliver Linux server binary
wget https://github.com/BishopFox/sliver/releases/download/v1.5.34/sliver-server_linux -O /usr/local/bin/sliver-server

# Make it executable
chmod +x /usr/local/bin/sliver-server

# install mingw-w64 for additional capabilities
apt install -y mingw-w64
```

After the processes finish, use the following command:
```
# Create our future working directory
mkdir -p /opt/sliver
```

Once we have done this, we are now ready to observe the noise from out two machines interacting! We will continue this walkthrough in Part Two - Observing Noise! 
