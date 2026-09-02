## Crashes after Updates

This is just disgusting.

> This Informational topic Assumes you know what the SSDT (Syscalls) mean and Assumes you have some broad knowledge about Anti-Cheats.
> And it assumes that you did read the BugCheck analysis Manual.

---

## Fundamentals

Kernel Drivers were the Most Privileged Executors in the OS.

Windows Guarantees multiple things Which are:

- The Kernel Documents and The Includes what functions they contain what prototypes they contain.
- The things Windows or Microsoft DOESN'T guarantee is the Undocumented functions due to changes in its Properties and functionality.

Some Drivers do use these Undocumented Structures Especially Kernel level anti cheats which are referenced as (KACs) or (KLACs).

KACs are Notorious for using Undocumented Structures which in Return induce a Bugcheck and a High Blood Pressure.

Kernel Drivers typically use Undocumented Structures and they stay stable due to a Stale Mate situation.

**What is a Stale Mate situation?**

A Stale Mate situation is when a structure is so Critical it doesn't change.

Take the PS_PROTECTION structure in the EPROCESS structure for example.

The EPROCESS is just the Properties for a Process that alters how windows deal with it.

The PS_PROTECTION hasn't practically changed since 20H2 all the way to the current 25H2.

Because changing it would break MULTIPLE things in Windows Including LSASS or LSA.

> No Bugchecks would happen unless you have something hardcoded or improperly done because you need data retrieval which will give you Information you DON'T mess with.

That is a Stale Mate situation.

Now the game of Undocumented Structures and Documented is like a Russian Roulette.

I can guarantee that the Cylinder 1,2,3 is clear but I cannot Guarantee that cylinder 4, 5, 6 are clear.

Maybe there is a bullet maybe two cylinders have a bullet maybe all of these have a bullet.

Exactly. So who would be at fault when a Reliant Driver on Undocumented Structure that always change Bugchecks the machine? That Driver and Not Windows, because you broke the contract and shot yourself.

Some may argue:

**"This is Incorrect since if it was working before then it should keep working"**

Well no, you just Got Lucky with that specific structure before it changed. Microsoft Did not Guarantee it.

---

## My Machine is Crashing After this F*cking update for no Reason F*ck Windows! Linux is Better!

**(Some Random Linux Fan that follows Louis Rossmann I don't need to mention any names yall know yourselves)**

Before actually determining That you would need to prove a few things:

- The Issue is Reproducible without a Kernel level Driver.
- The Issue is Consistent.

The crash usually happens because from games Loading a Kernel driver as an Anti-Cheat and as I stated in the Fundamentals some Drivers Especially KACs.

So before you open that Crash Dump and Analyze it (Bugcheck analysis), first Think and Reason:

- Was it Bugchecking before?
- What is the Stop code of the Current BugCheck?
- Did the Bugcheck change?
- Is it Reproducible?
- Is it reliant on a Kernel driver to get triggered?

Because sometimes when the issue is with the IRQL (Interrupt Request Level) for example, it will show the faulting module as `ntoskrnl.exe` itself (which is the kernel) so Analyzing crash dumps is not always conclusive.

So just blindly trusting the Faulting module is stupid.

What I would recommend is analyzing the Stack text and understanding what it means until you find the illegal call. Sometimes the SYMBOL_NAME does show where the Bugcheck Instruction was executed but I wouldn't trust it.

You'd need to Read the stack text and determine what is wrong the Stack Text is everything but even when it is Everything it still may Mislead.

So my Advice is Compare older behavior to newer behavior instead.

Like for Example:

The game deployed EAC (easy anti cheat) while I was playing War Hammer Vermentide II. It didn't Bugcheck my machine once. But after I Updated Windows the Machine does trigger a crash routine.

But does it trigger that crash routine without that driver being loaded?

- If it does then the fault is Windows's Fault.
- But if it doesn't and EAC for example is needed to Trigger that Bugcheck, then it is NOT windows but it is EAC.

This Informational Topic is rather a Manual on how to address faults accordingly because:

- If Microsoft changed a Internal Structure that it Didn't guarantee but EAC relies on it Then it is Not Microsoft's Fault because EAC broke the Contract on what is Guaranteed and what is NOT.

---

## What if the Issue was from a Faulting Driver that is Third Party?

Then Contact the Vendor. Provide them with your Diagnosis and Observed behavior, and also Provide the Crash Dump if one Was Produced.

---

## What if the issue was from Microsoft? (Less Likely, still happens anyway)

Contact Microsoft Support staff. Provide them with the Diagnosis, Observed behavior, and the Crash Dump.

---

If you have any critics or you found an Actual issue inside this Informational topic I would like to hear why.

Contact me Directly via Session or/and Raise an Issue on this Topic.
