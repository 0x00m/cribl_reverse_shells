# cribl_reverse_shells
Cribl Exec-Source Reverse Shells

This is purely for educational and research purposes ('Ethical Hacking'). Do not use this in any unauthorized environment as that is ILLEGAL. I do not hold any responsibility for you choosing to act illegally. 

Thanks to https://github.com/deeexcee-io for the Powershell Obfuscator.

# Requirements
1. Ability to create Exec Sources in Cribl through Management UI. (Authorized Access) 
2. A listener / attack box. 
3. Time and a touch of effort.

# Summary
This is a rather similar vector/method to Splunk Reverse Shells. Were going to abuse code execution "features" to get ourselves a shell.

# Process

To start, we will want to find ourselves in the "Edge" Portal / Management Interface (I believe this is also possible in "Stream", but I have only tested in Edge):  

  
![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/cc40a7dd-51b3-477e-a366-f00092cb43d5)


From there, you will want to filter your Victim/Target host into its own fleet utilizing fleet mappings. Were doing this to isolate the victim and not have however many grouped servers spam however many requests at your attack box. If this is confusing, I suggest taking an hour to review the basics of fleet management:
https://docs.cribl.io/edge/fleets/

Once we've seperated the host we'd like to attack, we will fumble our way over to adding a new Exec source. It should look something like this:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/d75c5f35-872f-4062-9afd-91168a4b4d20)

We even get a neat little warning at the top.

Lets click add new source and then manage as JSON:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/6c45751e-4457-4a35-8e38-24f7cd29364c)

Go ahead and copy the evilsource configuration into this new cribl source (Make Sure to Update the AttackBoxIP and Port to properly reflect your http server) and then press ok and save:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/730fd554-9c26-426a-b0bd-e56f7a239e96)

You should now see that a commit is ready to be deployed:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/414b7337-67c9-484b-af47-37a817af05ef)

But before we deploy, we have a bit more prep to do. 

Generate a powershell reverse shell script or rip one from the internet (Ethically, of course). I'm not a powershell wizard, but the ones from https://github.com/deeexcee-io or https://gist.github.com/guglia001/1de961b6b7fef4ef4f383015bb0f7c1e worked for me. Go ahead and write one of those ps scripts to a file on your attack box called badnews.txt

After thats written, start an http server to serve the script and start your listener on the port you generated the script for.

## Now I know what you're thinking

Couldn't we just use the command execution function of the cribl source to rip a one liner and not deal with this server garbagio? Yeah, probably. But windows defender seemed a little more scrutinizing toward some of these one liners and this was the way I moved around it. Feel free to innovate and find a more effective method, but if it ain't broke...

## Back to our regularly scheduled programming:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/6f7f4c4d-1b3c-4fc1-be27-969e4f52113b)

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/6bfd56b2-d997-4d6e-9d35-16a1b0346253)

With our http server up, script in place, listener open, and deployment ready to go, we press the commit and deploy button:

You can verify and hunt down your specific changes to not push anyone elses stuff and to maintain your "stealth" if moving silently:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/9f78094d-594b-4b70-be88-3d4b9a8ef459)

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/49740152-65e7-48b8-b581-d752e7e6ff7b)

And voila:

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/51f8337c-273d-48ab-a5fd-6225504215c7)

![image](https://github.com/0x00m/cribl_reverse_shells/assets/175245528/a2e61833-50e1-4e23-82ec-7161afae292f)

The default installation for edge nodes uses the LocalSystem account, but it is possible for this to be configured as a different user. 
