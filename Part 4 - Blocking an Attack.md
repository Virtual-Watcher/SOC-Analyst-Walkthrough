# Part 4 - Blocking an Attack

### Blog Post: [So you want to be a SOC Analyst? Part 4](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-1e0?sd=pf)

Note: You will need to complete "Part 1 - Virtual Lab Setup", "Part 2 - Observing Noise", and "Part 3 - Emulating an Adversary" or the blog equivalent to follow along with this part. Likewise, retrace your steps from Part 2 if needed. 

## Detecting Attacks

Setup your Linux and Windows VMs previously used, entering a Sliver C2 shell. Then use this command once the payload is active: 
```
shell
```

You should get output similar to this:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/32e71095-7205-4292-8934-afa7478ebdf4)

Now we have a System shell on our Windows VM. In the new shell, use the following command:
```
vssadmin delete shadows /all
```

If your VM is fairly new and made for these exercises, your output may be similar to the following:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/5ba0817b-4cd7-4331-88d5-89b47370a7f8)

This activity should still trigger an alert on our LimaCharlie Sensor. Run the following command to see if we still have an active system shell:

```
whoami
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/8bc7e09f-7f0e-4c2f-beac-4b21d612aeac)

Now, switch over to the Detection tab in LimaCharlie.

If our rule previously made was correct, it should detect activity similar to this:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4277b64e-a08b-477a-9fbc-f49a256559f1)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/2a02a277-884e-4216-a0b7-f4fdd4d6de89)

Click the specific activity to expand the detection:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/f3627022-376c-4fc8-b9ad-2632de6e1ae3)

Select "View Event Timeline" in the offending event:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/152944b5-4eb0-465e-bce3-68888968e650)

This will allow us to see the raw event that resulted in this detection:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/3b3b1629-3575-4199-9bcf-6012cee9b00c)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/90de5d97-99a5-47f4-bfbb-ec4ed06ef918)

We then need to select "Build D&R Rule", located in the top right of the raw event: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/2f80a914-b922-4bbd-8f1c-eec822489616)

Doing this will provide us with a detection template, from which we can craft our response action: 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/04c6969b-f706-4c03-a15f-d7260a11d76f)

Add the following Respond Rule:
```
- action: report
  name: vss_deletion_kill_it
- action: task
  command:
    - deny_tree
    - <<routing/parent>>
```

BEFORE

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/9fe2a63b-d4e5-4ca3-bda9-2d629a5a2e18)

AFTER

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/500b8295-eb5d-4aa1-a240-111b0dde5645)

Now save the rule as "`vss_deletion_kill_it`"

## Blocking Attacks

Return to the Silver C2 Session and run the following command:
```
vssadmin delete shadows /all
```

The command should appear to execute but will trigger our D&R rule:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1fb35430-4f91-46d7-83ef-c54f928a5493)

To see if the rule was activated, use the following command to see if we have an active system:
```
whoami
```

If our rule is successful, we should get similar output to this, exiting the shell or having it hang:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/e7eff3ab-4191-4dda-9584-345dbfa985fa)

Now since we have learned how to block an attack, we will move on to my last section covering my personal journey through Eric's blog series, "Part 5 - Triggering YARA Scans".
