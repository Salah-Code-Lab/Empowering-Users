## Analyzing Executables (Advanced)

> This Manual Assumes you did Read or at least you know how to:
> - Analyze Bugchecks
> - Attach WinDbg to VMWare and know how to use it
> - Setup Virtual Machines and Snapshots in VMWare
> - (Not Present in my Manuals): Know how to use some of Sysinternals Tools (Note They are Unneeded but some tools are good such as DbgView, ProcExp)
> - And Assumes you know some C

> If you don't know any of these I Heavily Urge you to learn about them first.
> Start with the Analyzing Bugchecks first then Type II hypervisors then WinDbg Usage.

> This manual doesn't mention DIE or Detect it Easy, or YARA rules since they aren't really Necessary to dictate if a Executable is Malicious follow the manual and you will understand.

---

## Fundamentals and Before we Begin

Analyzing Executables taking it to a another Level doesn't Nullify not using the Basic Methods because they are still Critical.

For Example:

A Malware that Queries the Registry Keys to determine if there was a Hypervisor (You don't know that yet) can exit upon detecting one.

And I am talking about Any Hypervisor (VBox, VMWare, Hyper-V(Type II), QEMU, Tria.ge, etc etc).

So how will you know if you went straight to VMWare? You wont not after some time wasting.

But when you go with the Basic Procedures such as Triaging the Sample in Tria.ge, VirusTotal.

You are Essentially doing 2 critical things that will determine your analysis:

1. Gathering as Much data as Possible.
2. Gather as much Context as Possible.

And the Third which Supports Context directly:

3. What is the Executable Origins? A Tool? Pirated? Unknown? etc etc.

These are the three Pillars of Analyzing any Executable.

And there are Rules that you must follow.

---

## Do not Early Out (Rule No.1)

While yes Software Querying Certain keys to determine if it was in a Analysis Environment seems Suspicious.

But what if it was a game? Debatable but the Software is Legitimate.

That is why you need Context not just Data.

---

## Exceptions

The only Exception to the **Do Not Early Out Rule** is if Malware or a Software Exits Right away after detecting a Hypervisor or a Virtualized Environment.

Now you aren't denied from using Real Hardware but you will waste a Crap ton of time doing so and it is not a Resource that is Always Available.

---

## Data Can Overflow Context (Rule No.2)

Even if the Software seems Legitimate If it executes other Actions that are Malicious without any Further Doubt it is Malicious.

Having a False Positive (FP) is 100% better than having a False-Negative (FN and no I don't mean the FN key on the keyboard).

This rule has no Exceptions.

---

## What to Do after gathering as Much data AND context?

You need to think and Understand what the Executable is doing.

Because Tria.ge does Flag Even Legitimate Operations as Malicious.

For Example:

A Repartitioning tool Opened a Handle to `\\.\PhysicalDrive0` or `\\.\PhysicalDrive1` to Either Repair or Repartition the drive.

Tria.ge will flag it as a Bootkit but it isn't.

That is why you need to understand the Data that you have and then Comparing the Context with it.

For Example:

- The Repartitioning Tool Is opening a Handle to the Disk to Repartition the Drive that is Normal.
- The Origins of the Executable is stated and Documented by the Author.
- The Author is Reputable and trusted.

Then this tool MAY BE legitimate.

That is when you Move on to VMWare and Analyzing the Behavior.

---

## Drivers

These are a Must when the Analysis is Stagnant or you need the Actual Dumps as Proof of what your sample writes.

Stagnant I mean You aren't observing anything Visible during the Analysis you ran the sample and nothing seemed to happen.

Exactly that means the sample is Stealthy and you need to force it out of its Cover.

For the Writes and Dumps I once used MBRFilterPP to Dump what Petya.A wrote to the Bootsector and you can see the analysis of it [here](https://github.com/Salah-Code-Lab/Petya.A-Documentation) in a file called Doc.

Now why did I assume you know a bit of C?

Well if you needed to add a entry, edit a rule, edit a DbgPrint call to include more information, etc etc. Sometimes it is not needed but sometimes it is.

---

## Recipe for Success

Get your Virtual Machine Ready by starting it up and **DISCONNECTING THE INTERNET**.

You do not know if what you are analyzing is a Worm or not So shutoff your internet on Guest not host.

Your VM is Ready? Good now begin gathering the data by Running the [basic Procedures](https://github.com/Salah-Code-Lab/Empowering-Users/blob/Basic/Analyzing%20Executables.md).

After running the Procedures, Understand what your Data Means and what your Context is while Applying the two rules I gave you.

If the analysis is Non Conclusive and Doesn't Query or search for a Hypervisor then move on to your VM.

Before You go ahead just take this note:

If the sample causes a BugCheck on the Virtual Machine Attach WinDbg so you can Analyze the Bugcheck.

It is Good Practice to attach WinDbg afterall but make sure the Hypervisor on Host is Disabled.

If nothing seems to happen You'd need to Escalate Further by using Drivers.

Mine exist and in their Analysis Version You'd need to Break in WinDbg then run:

ed nt!Kd_DEFAULT_Mask 0xFFFFFFFF

To Load you'd need to:

1. First Have Secure boot Disabled.
2. Second Have Testsigning on Via:

<code>bcdedit /testsigning on</code>

You will have the `.inf` to install the drivers easily.

Just Right Click the Inf and click on install then install anyway then reboot.

While Analyzing Consider having ProcExp as well just keep a side eye on it sometimes a tool that seems basic in the right time can make a lot of differences.

Or the Right tool in the Correct time and Place can make a difference in your analysis be it Custom or one that you found.

And from there the Massacre of the Malware begins:

- If it edits any BCD Variable you'd Know from [BCDG].
- If it executes a illegal write on Disk you'd know from [MBRFPP].
- If it executes a Illegal Registry Write you'd know from [RegF].

Gather the data they gave you and understand again what it means to get to a Conclusive answer.

> **Just Note:** I didn't give you any Example I know you noticed but I didn't because this skill Grows with mistakes and execution not examples you need to grow your OWN way of thinking.
> But if I would recommend a way? Don't jump into scenarios or conclusions Right away that is my advice.

