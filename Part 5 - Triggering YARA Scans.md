# Part 5 - Triggering YARA Scans

Note: While this section is my Part 5, in the blog series this section mostly covers Part 6. Though, I will provide the link for Part 5 of the blog series below as it is a useful resource in learning how to tune false positives with your detection rules. 

Addtionally, to follow along with this section, it is important to have at least completed Part 1 and 2 of my walkthrough or the blog equivalent. 

### Blog Post: [So you want to be a SOC Analyst? Part 5](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-54f?triedRedirect=true)

### Blog Post: [So you want to be a SOC Analyst? Part 6](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-7ed)

## Detecting Sliver with YARA Scans

We are going to tailor a YARA Scan to detect Sliver. in LimaCharlie, navigate to "Automation" and under it "YARA Rules"

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/98df3158-2d60-4d6f-aea5-c63b7fd79458)

Then click "Add Yara Rule":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/caa493d4-aa1b-4a5f-974c-4412deddee25)

Name the rule "sliver" and copy and paste the contents from this link provided by the blog to the rule block and save it: [gist](https://gist.githubusercontent.com/ecapuano/2c59ff1ea354f1aae905d6e12dc8e25b/raw/831d7b7b6c748f05123c6ac1a5144490985a7fe6/sliver.yara)

When done, your Yara Rule should look similar to this, if so save your rule: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/86e9310b-bd15-40b6-aa72-190f2543278c)

 Likewise, create another rule called "sliver-process" and repeat the steps from before, following this link: [gist2](https://gist.githubusercontent.com/ecapuano/f40d5a99d19500538984bd88996cfe68/raw/12587427383def9586580647de13b4a89b9d4130/sliver_broad.yara)

 If successful, your new rule should look like this before saving:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f046bc16-e741-4811-b4d1-d3dfe57a8320)

Now you should have two Yara Rules: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/bfffd2b9-e604-4154-b8cb-de3aee9a6089)

Before using these rules, we need to setup some generic D&R rules that will generate alerts whenever a YARA detection occurs. Under "Automation", go to "D&R Rules":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/90a605d0-ee47-41ba-b08b-e4c95c1fd28d)

Create a new rule in the top right corner:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e2968342-afe5-4902-8fde-d3467ffaa83e)

In the Detect block, paste the following:
```
event: YARA_DETECTION
op: and
rules:
  - not: true
    op: exists
    path: event/PROCESS/*
  - op: exists
    path: event/RULE_NAME
```

Like so: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/36944bf8-6fca-407d-b6a6-623abab4e829)

In the Respond block, paste the following:
```
- action: report
  name: YARA Detection {{ .event.RULE_NAME }}
- action: add tag
  tag: yara_detection
  ttl: 80000
```

Like so:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f9ee6a30-f62d-4f9e-9cb8-ff89988df0ef)

Save the rule and title it "YARA Detection":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7ee32926-ddad-4526-9218-781cee773541)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f9180fc6-a6cd-48d4-b6ae-9a1ee4e7159b)

Create another new rule:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e0fa8119-9df8-44b0-ba3a-c7e9e9bc5f6d)

In the Detect block, paste the following:
```
event: YARA_DETECTION
op: and
rules:
  - op: exists
    path: event/RULE_NAME
  - op: exists
    path: event/PROCESS/*
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/649ef246-6d0f-4600-a74a-49d9c6896113)

In the Respond block, paste the following:
```
- action: report
  name: YARA Detection in Memory {{ .event.RULE_NAME }}
- action: add tag
  tag: yara_detection_memory
  ttl: 80000
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/341e59e9-e65d-48ba-978f-2b17d94fca7b)

Save the rule and title it "YARA Detection in Memory":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/ff59a50f-e13d-440c-91d6-43c0e3408009)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1e1f5e4e-7772-41b3-bc30-cc8bbdb95274)

In LimaCharlie, navigate back to the "Sensors List" and click on our Windows VM sensor:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/578c6ab3-5344-4d30-81a3-a7fab7f6d62f)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/97b28773-32ca-4adc-ad01-a07dd163bdae)

Access the EDR Sensor Console, which allows us to run commands

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a2af7126-fa4a-41ee-a975-ce5ce48f01e7)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/8c4a87e2-99cd-4a9c-93cd-6970043e762a)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7db28f69-6d5d-4fb8-8d0c-2f85be780f22)

Run the following command to do a manual YARA scan of our Sliver payload, replacing the payload name with your own:
```
yara_scan hive://yara/sliver -f C:\Users\User\Downloads\<payload_name>.exe
```

For example:
```
yara_scan hive://yara/sliver -f C:\Users\User\Downloads\MODERN_PHONE.exe
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0c60476e-e340-4d9b-85a9-8ab5d97a52c7)

Hit enter twice to execute the command. Output should be similar to the following if your Sliver YARA rule was successful:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/435542ee-a4e2-4126-8beb-0d1e98c923fb)

Now navigate to Detections:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/757eecda-1b20-459f-b5cd-51ad03757c46)

As we can see, we have our new YARA Detection as an entry:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/55eca502-0286-4fc0-994a-b213589df044)

## Automating YARA Scans to Detect Executables

To continue the exercise, let us make a YARA Rule that automatically scans processes launched from the Downloads directory.

Navigate to Automation and D&R Rules:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4c4e5c5e-c12f-47cb-a951-c3b1d3d1aa8c)

Create New Rule:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/ed81ffa2-a892-4a92-9e64-2a32156bf5cc)

In the Detect block, input the following:
```
event: NEW_PROCESS
op: and
rules:
  - op: starts with
    path: event/FILE_PATH
    value: C:\Users\
  - op: contains
    path: event/FILE_PATH
    value: \Downloads\
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/08b0cab4-bbe1-4b9a-9cf6-2c0796ab5de0)

In the Respond block, input the following:
```
- action: report
  name: Execution from Downloads directory
- action: task
  command: yara_scan hive://yara/sliver-process --pid "{{ .event.PROCESS_ID }}"
  investigation: Yara Scan Process
  suppression:
    is_global: false
    keys:
      - '{{ .event.PROCESS_ID }}'
      - Yara Scan Process
    max_count: 1
    period: 1m
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/3a85d88b-97d9-493f-81e0-d7aa242a222d)

Save the rule as "YARA Scan Process Launched from Downloads" as per the blog, or as I did "YARA Scan from Downloads" for short:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/eb92b374-467d-4803-8816-957fb5a21949)

Our new rule should now be ready to try out in action!

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/15e75b0e-111b-400e-9927-d3f155ff0b14)

## Automatically Scanning Downloads

Switching over to the Windows VM with our payload, we simulate moving our payload out of the Downloads directory to our Documents directory before moving it again back to the Downloads directory once more. 

We use the following command, replacing the payload with your own:
```
move C:\Users\User\Downloads\<payload_name>.exe C:\Users\User\Documents\[payload_name].exe
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f97562c7-436c-400c-97f8-ba57db46bdb9)

Now, reverse the directories in this new command to return the payload to Downloads:
```
move C:\Users\User\Documents\[payload_name].exe C:\Users\User\Downloads\<payload_name>.exe 
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/705dd9c7-c298-4239-bc3b-45f9ad3f1979)

Switching back to LimaCharlie, we navigate to the Detections tab to see if there are any changes.

NOTE: This may take a little time to refresh and show. 

Eventually, you will see a detection in the same directory, for example: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/d850bba4-3363-41b5-bcfe-faad24295e94)

Lastly, it is time to test our NEW_PROCESS rule made earlier to scan running processes launched from Downloads.

On our Windows Machine in an Administrative PowerShell prompt, run the following after replacing "payload" with your payload name without the .exe extension: 
```
Get-Process <payload_name> | Stop-Process
```

After, use the following command to execute your payload once more: 

NOTE: This may take a moment to register.
```
C:\Users\User\Downloads\<payload_name>.exe
```

If successful, you should have similar output in the Detections tab:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f5eecc6b-5a9c-4817-9f3b-add7d3d96729)

And so you have it! Thank you to those who read or followed along as well as Eric Capuano for making this series free and available to everyone! Good luck in your studies and remember to always keep learning! 
