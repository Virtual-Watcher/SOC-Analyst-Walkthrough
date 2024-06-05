# Part Three - Observing Noise

### Blog Post: [So you want to be a SOC Analyst? Part 3](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part-77e)

Note: You will need to complete "Part One - Virtual Lab Setup" and "Part Two - Observing Noise" or the blog equivalent to follow along with this part. If needed, retrace your steps from Part Two. 

## Emulating an Adversary

Either on your Ubuntu Server or your Linux VM through a SSH session, start a C2 session on your victim Windows VM.

Run the following command on the Windows VM, the victim host: 
```
getprivs
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/d51ea939-241f-497e-83c2-7c213ee91b32)

Specifically, we are looking for if our user has the SeDebugPrivilege, as it will allow us to proceed as an attacker:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0494712f-6f96-43a7-8be9-11b06ad7e1fe)

Following along, we replicate a command for stealing credentials:
```
procdump -n lsass.exe -s lsass.dmp
```

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/307e23e9-d268-46c5-a6af-69c5492bc1e8)

This may result in an error if you did not launch your payload with admin rights or with an RPC error, but the main reason to execute this for our practice is to detect it through LimaCharlie. 


## Detecting an Adversary 

Switching over to LimaCharlie, we access our Windev Sensor and access the "Timeline" tab. In the quick search tab, we search for the keyword "SENSITIVE":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/14cd3114-da2f-4b1b-9e78-ef3f64012f7c)

If we select one of these instances, we can examine it more closely and find the "SOURCE" and "TARGET":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/1f9ed7cd-74dd-406f-ba0c-4a1f2b56f49c)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/4e500e28-bc1c-4f94-bab7-8f2633914091)

Having this information of what an event appears as when credential access occurs, our next step is to craft a [detection & response (D&R) rule](https://docs.limacharlie.io/docs/detection-and-response) that will create an alert anytime this activity occurs.

Therefore, we click in the top right corner of the event screen on "Build D&R Rule":

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/28aac770-f120-45a9-9ce8-005d8d71f844)

In the "Detect" section of the new rule, remove all contents and replace them with with this:
```
event: SENSITIVE_PROCESS_ACCESS
op: ends with
path: event/*/TARGET/FILE_PATH
value: lsass.exe
```

BEFORE

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/da4eb609-6f32-4f1f-8e5e-39caf2061745)


AFTER

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/ccbb8abf-e961-4815-9fd3-fc54a8f527c2)

In the "Respond" section of the new rule, do the same and add the following information:
```
- action: report
  name: LSASS access
```

BEFORE

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/0ebc0cf1-f795-4cf6-b9d9-620a34d7f0eb)

AFTER

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/c0431eac-0bf2-4829-ba2a-c412f1e29261)

Now scroll down and navigate to Target Event. This shows the "raw event" we viewed in the Timeline:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/81d48c0d-9bff-432a-87c7-1bcb9f151862)

Scroll further down and you will see a button that says "Test Event". Go ahead and click it, to test our rule. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/a933dfa8-7d95-482b-bbd1-5206554ba6c2)

If successful, we should be presented with a match:

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/6a639eaa-c77c-4fe7-900d-5e4f8c0c1472)

Now, scroll back up and click "Save Rule". Then give it the name "LSASS Accessed" and make sure it is enabled. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/7099a5ae-047e-4a00-a6d2-8e5531848714)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/778c5708-68be-4a9f-803d-edc8e5dec772)

## Testing Detection Rules

To test this rule again, we will return to the Sliver Server console and use the same procdump command we used earlier. 

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/2b3a5260-84fa-4f9f-a698-c7f018179abe)

Now, if we switch back over to LimaCharlie and go back under our Sensor to the "Detection" tab, we can see that our rule has detected our activity once more!

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/b4d26244-1ff5-4704-bc70-f4dcfc67fb3e)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/19237150-6fb7-42d3-97b6-642d333ab261)

![image](https://github.com/Virtual-Watcher/SOC-Analyst-Walkthrough/assets/171607952/92e9b71f-448a-4df7-af4a-93a5d8f6af58)

If your rules are correct, you should recieve similar output with your detections picking up activity of your C2 payload. In the next section, we will learn how to deny these actions in Part Four - Blocking an Attack!
