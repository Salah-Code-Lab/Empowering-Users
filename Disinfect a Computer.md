## Disinfect a Computer

This manual is about disinfecting a computer from any malware, whether it is rootkits, stealers, persistent malware, Trojans, etc.

This manual explicitly handles executables run as Admin, which have the most potential to persist.

> If you want to know more about different rights and different tokens and what they are capable of, check out Local Executables.

## Tools

The only tool we are going to use that is external is Autoruns.exe (Sysinternals).

## Steps:

1. Before doing anything—before running anything or executing anything—shut off your Internet and have a local copy of this manual.

2. Before checking what is in Autoruns, before checking anything, check the Exclusions folder and clear whatever is in there. Clear everything, even if they were legitimate, for multiple reasons, which are:
{
1. The Exclusions folder is a folder that never gets scanned by Defender and never gets any malware remediated. (Malware can abuse exclusions by dropping itself into that Exclusions folder.)

2. When running the scans, which we will do when we move on, the scans will completely ignore the Exclusions folder—malicious or not, it won't care about it.
}

3. Run a Microsoft Offline scan. Do not underestimate this step; you do not know how good it actually is sometimes.

4. After finishing the Offline scan, immediately go to Defender again and run a Full scan.

5. Steps related to Defender are now finished; the rest now is manual.
While Defender runs the full scan, launch Autoruns.
From there, we will do multiple things that are complicated, BUT they are simple enough for any casual user to think through them.

## Autoruns

Before executing or looking into anything in Autoruns, go to Options and uncheck "Hide Windows Entries" or "Hide Microsoft Entries" & "Hide Empty Locations". We want all entries shown.

1. Open up Autoruns and check mainly for Services, Drivers, DLLs, and Executables.

`A HEALTHY SYSTEM WILL NOT CONTAIN ANY EXECUTABLE, DLL, OR DRIVER LOADED AS A SERVICE FROM TEMP`

If you see an unsigned or unverified service, executable, DLL, or driver, before deleting it, search mainly for two things:

Before searching you can turn on your Internet now but be Quick
{
1. Its name, and see what comes up.

2. Upload it to VirusTotal and see what comes up.
}

If any of the two were violated, the thing must be deleted. A malicious executable has no business being a scheduled task or a service.

Now iterate through Autoruns: mark what is suspicious and delete what is malicious. Easy enough for any user—basically search and destroy.

## What This Clears Up:

This clears up almost every single type of malware, including some kinds of rootkits. But what it doesn't clear up is bootkits and other types of malware that start before the OS.

## Additional Notes:

You can run any other malware scanner like Malwarebytes, for example, but it is best to work locally without downloading any external service.



If anyone has any notes or questions or critcs or corrections id like to hear them in the issues
