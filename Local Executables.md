## Local Executables

This manual is about local executables—which include `.exe`, `.dll`, and `.sys` files—what they are capable of, the abilities standard and admin rights provide, and more.

## Standard Rights (MEDIUM_INTEGRITY_TOKEN)
(.exe, .dll)

Standard rights correspond to the **Medium Integrity Token**. 

The things it is capable of are many, but the most important core functions include:

1. Read/Write (R/W) access to files (`Desktop`, `Documents`, `Music`, etc.).
2. Establishing connections with external servers (C2 servers, etc.).
3. Writing to the HKCU hive (adding itself as installed software; cannot execute Local Privilege Escalation (LPE) unless a vulnerability is leveraged—*ahem, legacy hives, ahem*).
4. Consuming 100% of user resources (CPU, GPU, RAM).

Now, on paper, these permissions might not seem like they can go very far. 

But when you combine read/write access with an outbound connection to a C2 server? That is where it becomes diabolical.

Malware can encrypt your data even as a standard user, exfiltrate data to a C2 server, read cookies, steal passwords, and more.

Never underestimate the Medium Integrity Token. That is why you should check any file you run before executing it, because in less than a second it is capable of stealing all of your data and sending it to a C2 server.

Though this token may seem dangerous—because it is—it is present on every executable you double-click. This includes `firefox.exe`, `explorer.exe`, `notepad.exe`, and others.

However, this token is incapable of persisting on a PC unless it finds a vulnerability (*LEGACY HIVES, AHEM, AHEM*) or other vulnerable software.

The counter to it is mainly just not downloading any executable you see. But if you can't and you just have that urge, at least:
* Have CFA (Controlled Folder Access via Defender) enabled.
* Check it on VirusTotal.

As for `.dll` files, they can be executed via any other executable and will inherit the parent token (Medium Integrity or High Integrity Token [Admin]). 

This is why "I didn't run any .exe" is not a defense. If a program you trust loads a bad DLL, the trust is inherited.

## The Bottom Line

Standard user malware can't survive a reboot without a bug or your help.<br>
But it can steal everything you care about in the time it takes you to read this sentence.<br>
Check before you click.<br>
CFA on. VirusTotal if you're unsure.

## Admin Rights (HIGH_INTEGRITY_TOKEN)
(.exe, .dll)

> "If an attacker can execute code, modify the OS, gain physical access, or upload files to your system, you have lost control of that system." — Scott Culp

This is where it gets extremely serious. An Admin token is capable of doing about everything, including escalating to a higher token.

A High Integrity Token is the equivalent of having the ability to completely take over the whole OS.

Admin rights are capable of:
* Establishing persistence
* Escalating to a higher token
* Loading drivers (`.sys`)
* Modifying and manipulating the boot configuration data (BCD)
* Writing the bootmgr
* Wiping drives
* Writing bootcode
* Writing system files
* Setting handles (setting itself as a critical process that cannot be terminated; if terminated, the system will bugcheck with `0xEF`)
* Accessing each and every file
* Uploading files to your OS
* Taking full control of your system
* Modifying the OS
* Deleting critical folders
* Encrypting the MFT (only under certain circumstances, like Petya, for example)
* Mounting hives
* Accessing the whole registry

This is what any process is capable of once you click "Yes" on the UAC prompt.

## SYSTEM Token
(.exe, .dll)

> "Admin asks permission. SYSTEM doesn't need to."

Admin processes can escalate to this. `psexec` is a live example (a legitimate Sysinternals tool used to obtain system rights). 

This is basically what an 80% capability looks like. While it inherits almost the exact same handles as Admin, it has increased access, an increased ability to load drivers, and many other abilities.

## Kernel
(`ntoskrnl.exe`, `hal.dll`, `.sys` files)

This is the highest privilege point that has 100% rights to do each and every single thing it desires. 

That is why many malware authors load a vulnerable driver to have these kinds of rights, which happens with drivers and mostly KACs (kernel-level anti-cheats). Ironically, the software meant to stop cheaters often runs at the kernel level with vulnerabilities that malware abuses to get there too.

Note that BYOVD (Bring Your Own Vulnerable Driver) attacks are sometimes limited to R/W primitives and not a full kernel takeover like an actual rootkit.

## What Can I Do About It?

Just don't run any executable you see—as easy as that.

Use a tool named `ConfigureDefender.exe` and configure Defender as **High** (not **Max**, as it causes too many false positives).

Use CFA wisely and rigorously.

And most of all: if any individual tells you to download this `.exe` or that `.exe`, do not install it or download it at all. I am pretty sure you will listen to me after seeing what standard and admin rights are already capable of :D. 

Not even a server—do not trust anything that tells you to download an `.exe` to play a game. 

If you really need it:
* Search for its reputation.
* Make sure the source is reputable.
* Run it on VirusTotal.

**For better analysis**
Go to Analyzing Executables
