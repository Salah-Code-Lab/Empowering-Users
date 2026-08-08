## Analyzing Executables

This manual is complex, and I know it might be hard to digest the first time through. But we are all human—we learn. That is why I am here. I learned, so I want to share my knowledge as much as I can. Not because I have to, but because I want to. I am sick of malware, sick of scammers, and sick of seeing people pay thousands of dollars for something that should be a basic right: **PROTECTION**.

Either way, enough backstory.

Analyzing executables is a crucial skill for determining whether a file is safe. Doing so requires a few tools some advanced, some basic.

## Tools

1. **VirusTotal**
2. **Tria.ge**

That is really all you need to determine if an executable is malicious. These tools catch about 90% of malware. I’ll be honest: for the remaining 10%, it comes down to using your brain, analyzing the behavior, and applying basic common sense.

### Step-by-Step Triage

First, after downloading or receiving the executable, **do not launch it**. 

Simply take the file and upload it to VirusTotal. If detections pop up, investigate:
* What were the specific flags?
* Are they related to ransomware, infostealers, or trojans?

Make a note of whatever it was flagged for, then move on to **Tria.ge**.

### Understanding MITRE Vectors

There are a few key concepts you should know, and I promise to keep them simple starting with **MITRE vectors**.

To put it simply: **MITRE vectors are just records of what system settings a file changed, modified, or queried that caused it to be classified as a specific type of malware.**

That's it. Not so scary, right?

### Using Tria.ge

1. Log into Tria.ge using your email and submit your sample executable.
2. Select at least 3 Windows versions for the sandbox run making sure one of them is the latest build (Windows 11). The rest can be older builds (e.g., Windows 10).

Now, lock this rule into your core evaluation logic:

> **If a process queries VMware tools, VirtualBox, QEMU, or Motherboard/BIOS details specifically to inspect its environment (what I call *Environmental Awareness*) consider it malicious right away.**

Even if it claims to be a legitimate utility, querying that kind of hardware telemetry without a functional reason indicates anti-analysis behavior. It is gathering data it has no business requesting, often harvesting hardware information that belongs to **you**. 

*(Note: Services like Steam's Hardware Survey are an exception because they explicitly ask for your consent first and only collect high-level hardware specs.)*

## Score

after a executable had been analyzed it will have a score out of 10 
and no ladies and gentlement that is not your math grade exam it is a score for how malicious executable is
* sometimes it scores a 10 while it does nothing 
* sometimes it does while doing everything 
so common sense just basic is needed on this one

### The Reality of Modern Software

Unfortunately, some legitimate commercial software and game clients query motherboard serials when they don't need to, or inspect currently loaded kernel drivers to map out the running environment. 

They do this to execute adaptive behaviors like Corrupting windows kernel to the point of it not being able to bugcheck in VirtualBox from a kernel driver, immediately exiting inside a Tria.ge sandbox, or throwing fake critical errors inside VMware.

Sadly, that is where the state of software is today. Certain applications actively harvest user telemetry sometimes even from minors evem when there are privacy regulations. 


## The Bottom Line

That is the core methodology for analyzing an executable. 

While there are many other advanced reverse-engineering, and detection techniques, this approach is designed for everyday users, not malware analysts. Casual users don't need to spin up custom local hypervisors just to figure out if a file is safe they just need an answer. And knowing if a file is safe is their right.
