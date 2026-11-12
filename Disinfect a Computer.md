## Disinfect a Computer

This manual is about disinfecting a computer from any malware, whether it is rootkits, stealers, persistent malware, Trojans, with the exception of bootkits, and some rootkits. 

> Note: Stealers may already have stolen your data if they were present but to cut them off is better, remember to change your passwords, and recover your accounts from a Clean machine. 

> If you want to know more about different rights and different tokens and what they are capable of, check out: [Local Executables](https://github.com/Salah-Code-Lab/Empowering-Users/blob/Basic/Local%20Executables.md).

## Tools

The only tool we are going to use that is external is Autoruns.exe (Sysinternals).

> You can use other Third party tools like Anti-Viruses such as: Malwarebytes, or Kaspersky for example.

## Steps:

1. Before doing anything—before running anything or executing anything—shut off your Internet and have a local copy of this manual.

2. Before checking what is in Autoruns, before checking anything, check the Exclusions within windows defender and clear whatever is in there. Clear everything, even if they were legitimate, for multiple reasons, which are:

* The Exclusions folder is a folder that never gets scanned by Defender and never gets any malware remediated. (Malware can abuse exclusions by dropping itself into that Exclusions folder.)

* When running the scans, which we will do when we move on, the scans will completely ignore the Exclusions folder even if malicious, it won't care about it.


3. Run a Microsoft Offline scan. Do not underestimate this step; you do not know how good it actually is sometimes.

4. After finishing the Offline scan, immediately go to Defender again and run a Full scan.

5. Steps related to Defender are now finished; the rest now is manual.
While Defender runs the full scan, launch Autoruns as an Admin.
From there, we will do multiple things that are complicated, BUT they are simple enough for any casual user to think through them.

## Autoruns

Before executing or looking into anything in Autoruns, go to Options and uncheck "Hide Windows Entries" or "Hide Microsoft Entries" & "Hide Empty Locations". We want all entries shown.

1. Open up Autoruns and check mainly for Services, Drivers, DLLs, and Executables.

> Note: its best to mark the Executables or Dlls you find in Temp for Triage.

`A HEALTHY SYSTEM WILL NOT HAVE A DRIVER LOADED FOR EXAMPLE FROM TEMP`

If you see an unsigned or unverified service, executable, DLL, or driver, before deleting it, write down the

* Directory its Located in 

* Name of the Binary 

Keep searching until you finish all of the Entries within Autoruns, when finished Turn on the Internet and begin to triage what you've found. 

Such as:

1. Its name, and see what comes up.

2. Upload it to VirusTotal and see what comes up, Specifically Detections.

If you cannot determine if it is malicious or not you can use an Online sandbox such as [Hatching Triage](https://Tria.ge). 

If any of the above returned Positive results for being Malicious, the thing must be deleted. A malicious executable has no business being a scheduled task or a service.

* If its a Service you can stop it via <code> sc stop `service name` </code> then <code> sc delete `service name` </code>

<mark>Though be careful since the service can be tied to a Kernel driver, critical service, or other legitimate executables so double check before you delete any services</mark>

* If its a Process you can Terminate it if its running via Task Manager but if it cannot be terminated for some reason you can boot to safe mode and continue from there!



## What This Clears Up:

This clears up almost every single type of malware, including some kinds of rootkits. But what it doesn't clear up is bootkits and other types of malware that start before the OS.

`Please note: that Windows Defender may not detect all threats that is why there is Manual triage but even then Malware can still exist but most of the time it works by 100% on clearing up the threats in many scenarios`

## Additional Notes:

You can run any other malware scanner like Malwarebytes, Kaspersky, for example because some Anti-Viruses are genuinely better than Microsoft Defender.


If anyone has any notes or questions or critcs or corrections id like to hear them in the issues. 

> Contact me: 056bf8ea1a057b4f351d8b651944252cd4d88416ce6c11761f0c406f228a302301 (Session)
