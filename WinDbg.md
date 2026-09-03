## WinDbg Usage

> THIS MANUAL ASSUMES YOU DID READ THE TYPE 2 HYPERVISOR MANUAL AND THE BUGCHECK ANALYSIS MANUAL

WinDbg is a Very useful tool and Sometimes vital in some cases such as:

- Analyzing Bugchecks (Reference the Bugcheck Analysis Manual)
- Patching live in memory (Via commands)
- Debug Drivers
- etc

WinDbg is the Lifeline basically in some cases whether diagnosing issues or analyzing or just patching random stuff with it (its fun)

---

## Fundamentals

WinDbg doesn't only Analyze Bugchecks For your Information.

It Also can debug the kernel Live while its booting up and allows you to inspect the Whole RAM either when you Break (Will be explained) or Bugcheck (BugCheck Analysis manual, Trigger Bugchecks at will via WinDbg).

Before we dive into these things i must tell you a few things.

To actually get it to work in a VM you Need VMWare (Reference the Type 2 Hypervisors).

Boot to your VM and Execute these Commands.

---

## BCD Config

The BCD Config is what will allow us to attach WinDbg In the first place so lets begin.

First we need to allow the Debug so Get yourself a Admin CMD and run:



<code>bcdedit /set debug on</code>

That will give us the whole capabilities to debug now we just need to Configure it properly.

So Run:

<code>bcdedit /dbgsettings SERIAL DEBUGPORT:1 BAUDRATE:115200</code>



This will be the Port that Windows and WinDbg allows them to communicate with eachother.

After wards this Option is OPTIONAL and read my WARNING TWICE.

This is ABLE to make your machine Unbootable unless you trigger Recovery. But in some cases yes it will eventually time out but in some other cases it wont allow boot until WinDbg attaches in the first place.

**READ THAT WARNING TWICE!**

Alright:

<mark>bcdedit /bootdebug on</mark>
> Use with caution 


**Note:** Secure boot must be Off and i recommend turning it off reference the Type 2 Hypervisor Manual for more Information.

Now that you did set the Virtual machine with the Device needed for communication, change the Serial Port settings to be:

- Connected at Power on Check
- Use named Pipe
- `\\.\pipe\pipe1`
- This end is a Server
- The other end is a Application

Now go to WinDbg on Host and open it normally.

Go to:
`File -> Kernel Debug -> COM ->`

Enter these values in:

- Baud: `115200`
- Port: `\\.\pipe\pipe1`
- Reconnect: Check
- Pipe: Check

Then Click on Ok.

Then Reboot the Virtual Machine and it should attach it would give you a value like:


Connected to Windows 10 26100 x64 target at (Mon Aug 24 23:42:18.619 2026 (Timezone)), ptr64 TRUE
Kernel Debugger connection established.
Symbol search path is: srv*
Executable search path is:
Windows 10 Kernel Version 26100 MP (1 procs) Free x64
Edition build lab: 26100.1.amd64fre.ge_release.240331-1435
Kernel base = X PsLoadedModuleList = Y
System Uptime: 0 days 0:00:00.000


Though it may differ from VM to VM and some values may differ on each reboot due to KASLR measures.

Now from there you can Go NUTS.

To Execute any command you must interrupt the VM and break.

---

## Breaking

Break isn't a Relationship its not like your relation with your GF lmao.

Break means to break out of that state and stop Executing instructions.

**Note:** (It doesn't actually stop all execution there are still some instructions that get executed to break out of state just putting it out there).

To break out of state In WinDbg:

Go to Debug and you will see an option called Break.

And when it breaks it will Notify you happily:


*******************************************************************************
*                                                                             *
*   You are seeing this message because you pressed either                    *
*       CTRL+C (if you run console kernel debugger) or,                       *
*       CTRL+BREAK (if you run GUI kernel debugger),                          *
*   on your debugger machine's keyboard.                                      *
*                                                                             *
*                   THIS IS NOT A BUG OR A SYSTEM CRASH                       *
*                                                                             *
* If you did not intend to break into the debugger, press the "g" key, then   *
* press the "Enter" key now.  This message might immediately reappear.  If it *
* does, press "g" and "Enter" again.                                          *
*                                                                             *
*******************************************************************************
nt!DbgBreakPointWithStatus:
fffff801`b1efa090 cc              int     3


Any Break out of state instructions that get executed will be:

nt!DbgBreakPointWithStatus:
<Some address> cc int 3

And the same thing happens when a Bugcheck routine is executed it will tell you:

The system encountered a fatal error use `!analyze -v`.

nt!DbgBreakPointWithStatus:
<Some address> cc int 3



And when that happens you have the FULL freedom to inspect RAM In the current state and the instruction set.

(Though not full visibility there is a difference between Visibility and accessibility which we have).

---

## Other Functionalities

Now WinDbg Does Offer Multiple other things to Execute at will.

You can Patch Tables directly via:

ed <target address> <value of that address that is needed>


And yes it can be done with SSDT tables though it will cause a 0x50 if done improperly.

And even when it is done Properly it will cause a 0x109 later which is a Crash induced by PatchGuard.

You can also crash the Target OS with:


gn (Go Unhandled exception, Triggers 0x3D)

or 

.crash (Triggers MANUALLY_INITIATED_CRASH or 0xE2)


Along with Multiple other Functionalities this is just the surface which i use.

There are multiple other things that you will use other than these and it is up to you to explore i told what i had verified.

And this is Simplified WinDbg.

> **Note:** VMWare Utilizes WHP which would Make the Hypervisor of your Virtual Machines Hyper-V not VMWare itself.
> This will Reduce Performance in the VM and in some cases when WinDbg is attached it will cause an NMI Hardware Failure in the Guest not the Host.
>
> To disable the Hypervisor on host you need to do Multiple stuff that are UNDER YOUR RISK.
>
> You need DG-Readiness tool and `bcdedit /set hypervisorlaunchtype off`.
> I gave you the way you figure out the rest.

If there was any issues or Critics please Tell me right away by either Directly Contacting me via the Session ID or/and Raise an issue on this Specific manual and not the Whole Repo XD.

> Session ID:056bf8ea1a057b4f351d8b651944252cd4d88416ce6c11761f0c406f228a302301
