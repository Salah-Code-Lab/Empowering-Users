## Bootkit recovery


## What is a Bootkit? 

A Bootkit is a program that opens a handle to the boot drive (usually `\\.\PhysicalDrive0`) and writes malicious code to the first few boot sectors. This forces the system to execute its binaries first during boot before handing off execution to the Operating System.


## This Manual what does it help against ? 

Normal and advanced bootkits. Unless if the bootkit had Wiped/Encrypted the Master File Table, then this manual will be no good for you. If it was just standard writes to boot sectors, you can move on; if it was related to Filesystem damage, then as I said, this manual will not do you any good.


Advanced bootkits here I mean bootkits that persist via sectors, not NVRAM or in Firmware, just to be clear.


And one thing which is, if you mess up the partition map...

NOTHING TO YOUR DATA WILL HAPPEN unless if there was something external happening, but even then be careful.


Testdisk (the tool we will be using later) Doesn't write your data at all. It just writes:

- How to deal with this storage type

- and not: What is in that storage type


Just play it carefully because I destroyed 10 different machines and recovered them just for the sake of creating this manual. You just need simple thinking and you will be through.


## Before We Execute Any Recovery Procedures 


You have to understand a few low-level concepts first. I will explain them as plainly as possible.


### 1. Legacy BIOS (MBR)


BIOS stands for **Basic Input Output System**. Disks on BIOS systems are typically partitioned using **MBR** (Master Boot Record).


BIOS firmware reads execution code directly from the very first sector of the physical disk: **Sector 0 / LBA 0** (Logical Block Addressing).


**What is a sector?**

Simply put, a sector is a fixed-size block of bytes. Legacy hard drives use **512 bytes** per sector (though modern Advanced Format drives use 4096-byte native sectors). 


On MBR disks, **Sector 0 (512 bytes total)** is split into three distinct parts:

1. **First 446 Bytes:** Real-mode executable bootstrap code that initiates the boot handoff.

2. **Next 64 Bytes:** The Partition Table. It holds up to four 16-byte entries defining primary partition boundaries. ($16 \times 4 = 64\text{ bytes}$). This is why MBR natively supports a maximum of 4 primary partitions.

3. **Last 2 Bytes (Bytes 510–511):** The Boot Signature (`0x55AA`). Without this signature at the end of the sector, the BIOS will refuse to execute the boot code.


$$446\text{ bytes (Code)} + 64\text{ bytes (Table)} + 2\text{ bytes (Signature)} = 512\text{ bytes (1 Sector)}$$


### 2. Modern UEFI (GPT)


UEFI stands for **Unified Extensible Firmware Interface**. Disks on UEFI systems are partitioned using **GPT** (GUID Partition Table).


Unlike BIOS, UEFI **does not execute raw assembly code from disk sectors**. Instead, UEFI firmware contains built-in filesystem drivers (such as FAT32) to read files directly from disk partitions.


Here is how GPT layout is structured at the sector level:


* **LBA 0 (Protective MBR):** Kept purely for backward compatibility so legacy tools don't assume the drive is unpartitioned and overwrite it.

* **LBA 1 (Primary GPT Header):** Contains disk metadata, GUID identifiers, CRC32 integrity checksums, and memory offsets. It contains **no executable code**.

* **LBA 2 through LBA 33 (Partition Entries):** A dedicated 32-sector array holding partition boundaries.


Instead of running arbitrary real-mode sector code, UEFI parses LBA 1 to locate the **EFI System Partition (ESP)**, mounts its FAT32 structure, and directly launches compiled PE bootloader binaries like `\EFI\Microsoft\Boot\bootmgfw.efi`.


**Why does GPT support 128 partitions per disk?**

Each partition entry in GPT is strictly **128 bytes long**. 

If we take the 32 sectors allocated for partition entries on a standard 512-byte sector drive:


$$32\text{ sectors} \times 512\text{ bytes} = 16,384\text{ bytes total}$$


Dividing that space by the size of each partition entry gives us:


16,384 bytes / 128 bytes per entry = 128 Partitions


*On 4096-byte native drives, those same 16,384 bytes fit into just 4 sectors: 4 times 4096 = 16,384.*


## How to know the difference between UEFI and BIOS ?


So I imagine now you are conflicted.


You will know later when we use Test disk and other Utilities don't worry because the next few steps will be very telling.


### Required Tools:


1. **Hiren’s BootCD PE (HBCD):** A bootable WinPE environment packed with recovery utilities.

   * Go to the official [download page](https://www.hirensbootcd.org/download/), scroll down to the **Filename** section, and click the `.iso` link to download the image.

2. **USB Flash Drive:** Minimum **8 GB** capacity.

3. **Rufus:** A lightweight utility to write the HBCD image to your USB.


### Burning the Rescue Drive:

1. Insert your USB drive.

2. Open Rufus and select your USB device.

3. Under **Boot selection**, browse and select the downloaded HBCD `.iso` file.

4. Leave the default options and click **START** to create your bootable recovery drive.


## Recipe for Success 




## MBR


In MBR based Devices there are parts that are Flinicky and confusing but they are not so complex 

in a matter of fact they are very simple to understand 

And i forgot one thing which is 

yes you can only have 4 Partitions only in MBR 

but that goes for Extended and Primary 

not for Logical 

You can create a lot of logical partitions 

so if you had Data on some other partitions mark them as Logical


The Recipe for Success is: 


* Open Testdisk you will navigate with the arrow keys 


* Select your Main disk that you want to recover then hit Enter 


* For MBR devices you want to select / Highlight [Intel] Intel/PC Partition (I Know you thought about Intel the CPU company but it is something else)


* Select / Highlight Analyze and Hit Enter


* Then Select / Highlight Quick Search and Hit Enter and hit enter 


* Test disk will present you with a partition that cannot be recovered keep that in mind or write it down to identify it write down its size in sectors only


* Now when you want to recover the partitions use your Left and right arrow keys to select the partition type usually we make all Primary [P] But the System or Boot partition which is auto marked as * or [*]

Now remember that partition that TestDisk kept telling you about ? You do not want to mark it. but you should keep it as [D] or Deleted 

that Partition will Eat / Corrupt / overlap / Annihilate with the Recovery Partition or Other Partitions in Proximity which will corrupt some parts of that partition INCLUDING ITS DATA

and we are here either for Data Rescue or Restoring Boot Ability


* To know Which partitions from Which before we write anything Select a partition then press P on your Keyboard to list the files 

After Listing the files you will see what files Correspond to the Current Partition selected 

we need 3 Partitions selected as PRIMARY or in some cases two or even one i will explain later in the edge cases


These 3 Partitions contain: 

Boot/System Partition: (Has Boot Files and is automatically marked by testdisk as Primary Bootable or [*])


Primary/Main: (Has your Windows Install and your Data)

Now remember that Partition that Test Disk Told you it is Irrecoverable ? 

just don't mark it as Primary that is it as easy as that you can identify it but its sectors but keep it as [D] because even if you set it as logical it will overwrite data because it overwrites or eats other partitions which causes corruption

may be a PART of it Including the Recovery Partition by the way

it Contains the exact same files as a another partition they have Identical data so treat it as a Duplicate

If you did Write it down you can know which from which and Not select it to be recovered

but if you didn't ? 

then go to the Edge Cases Section don't worry Nothing is lost your data is still there not that something catastrophic happened


Recovery Partition:

has one unique folder which is 

Recovery and a small sector count you can know for sure by highlighting Recovery Directory/Folder then 

Pressing P again You will se a WindowsRE Directory you can highlight it then Press P again 

you will see a file which is Winre.wim 

if you see it 

then the partition is the Recovery Partition no Doubt left 


* Now after Knowing for sure which Partition from Which 

Select the Partition or Highlight them or whatever with your arrow keys 

then the left and right arrow keys to select Partition type 

now for these Two partitions which are the 

Main | Recovery 

They must be P or Primary

after selecting their type and the Structure is Ok 

move on to the next step


* If you have other partitions 


If you did have other partitions that contain valuable data 

Don't worry by default they don't overlap or conflict with others 

you can mark them as [L] or Logical and move on

BUT THE PARTITIONS THAT OVERLAP WITH OTHERS 

I AM WARNING YOU DO NOT TOUCH THEM THEY WILL CORRUPT THE OTHER PARTITIONS IN PROXIMITY

but keep in mind Main Partition/Recovery Partition must be Primary 

leave the System / Boot partition marked as [*] like how test disk did set it


## edge cases:

Now you maybe didn't mark / Write down the Partition that will cause other partition Corruption 

Do not worry the solution is easy 

See the Partition end ? in test Disk ? 

for example 

the Partition the Big one ends in say 500 for example 

just a number it ends at 500 

but the recovery partition starts at 300 and ends at 530 for example 

then the big partition as we know will just eat that partition and corrupt its data 

and don't worry you cannot accidentally write it if it already conflicts there wont be data loss 

But if you did write it alone without knowing then that data on that partition that was overlapping with the big may be already Burnt toast 


**What if i have one Partition that is marked with [*] what should i do then ? what if there is no Recovery Partition ? and there are no Other partitions but this one**

No need to worry 

because that singular Partition that is marked as [*] makes stuff easier on you and on me 

Write it and don't worry but make sure that it doesn't overlap with other partitions if there was any

if it was one partition just one partition that exists on Disk 

then it contains the BCD and bootmgr AND winre.wim 

which makes the commands straight forward 

for the BCD it is usually located at C:\Boot\BCD 

delete it (MAKE SURE THAT C:\ IS WHERE YOUR WINDOWS INSTALL IS AND NOT THE SYSTEM PARTITION) then run

bcdboot C:\Windows /s C: /f BIOS (ONLY IF THERE WAS EXACTLY ONE PARTITION IF THERE WAS OTHERS VERIFY WHAT KIND OF PARTITION THEY ARE THEN EXECUTE ON THE SYNTAX

bcdboot (OS Drive Letter)\Windows /s (System Partition Drive letter) /f BIOS

)

and for WinRE or Reagentc 

delete the file in 

C:\Windows\System32\Recovery\Reagentc.xml 

and then do 

reagentc /setreimage /path C:\Windows\System32\Recovery\Winre.wim 

if it was not there 

then make sure that there is a Recovery Partition that maybe you had missed 

If it was not found on a Separate partition or on the main partition 

i would recommend Backing up your data and resetting the PC 

because the environment can no longer be trusted because maybe the malicious actor is preventing recovery and other detection means aswell 

after making sure it is there and the path to the wim file is found 

then run 

reagentc /enable 

and you are done



* Hit Enter then navigate to [Write] Hit Enter then Hit Y 

Note: This DOESN'T WRITE YOUR DATA 

NOT DIRECTLY NOT INDIRECTLY IT ONLY WRITES SOMETHING CALLED THE PARTITION TABLE AND STRUCTURE 

NOT YOUR DATA

YOUR DATA MAY BE IN DANGER ONLY WHEN WRITING PARTITIONS THAT OVERLAP WITH OTHERS BUT THAT IS THE WORST CASE 

AND I DON'T WANT THAT TO HAPPEN 

I DON'T WANT THAT YOU DON'T WANT THAT NO ONE WANTS THAT


* Split 

Now if your Plan was to make the partitions accessible again so you can backup your data 

here we are done you do not need to proceed you can backup your data by plugging in the drive to a another computer 

then copying your valuable data 

if you want to restore Complete Bootability with Every Functionality possible including WinRE 

then Move on


* Boot again to HBCD 

after Booting back to HBCD or your USB drive that has HBCD

Now things get easier and more simpler now trust me 

what i want you to do is 

Know the Partition letter that Corresponds to which partition 

then write that down 

for example i have 4 partitions 

C:\, D:\, E:\, F:\ (Exclude the X:\ partition)

now on their own they are useless data they are just letters

but when you enter and check their files and know which from which is critical 

now access each Partition find where your Windows install is 

Say E: 

and find where the Boot Partition or system partition is 

just enter the partition and if you find bootmgr along with other files 

and the partition is About 50MB big 

before we proceed 

when you find that partition delete the file named BCD that is in it 

it is already corrupted and wont serve us any purpose

then from there we will be able to craft our command to restore the Boot Configuration data Otherwise known as the BCD 


now for example 

my Windows install is in E:\ and my System partition which has THE BOOT FILES remember boot files are different than your Main windows install for example is C:\ 

then we can type in our command 

bcdboot E:\Windows /s C: /f BIOS 

that will restore the boot config data 

But remember when you don't know the Partition letters it is more like 

Syntax:

bcdboot (OS Drive Letter)\Windows /s (System\boot partition Drive letter) /f BIOS

you must fill in the blanks tell where BCDBoot where your Windows install and boot partition is help it so it can help you

that is why i told you 

Identify which partition from which


* After Successfully running the command 

Now that you did run the command and it worked 

just a last step before we boot to Windows 

go to the Windows Partition and search for the file reagentc.xml 

it is usually in 

\Windows\System32\Recovery\reagentc.xml 

when you locate it delete it 


* Restart


* After Booting and ensuring that your Environment is clear and is not Compromised if Unsure Run [Disinfect A Computer](https://github.com/Salah-Code-Lab/Raising-Awarness-Empowering-Users/blob/main/Disinfect%20a%20Computer.md)


* Open up CMD as Administrator or with HIGH_INTEGRITY_TOKEN rights 

then the path to the winre image remember it 

\Recovery\WindowsRE\Winre.wim 

with the partition letter in the beginning 

then we would need to run 

reagentc /setreimage /path (Drive Letter for the recovery partition)\Recovery\WindowsRE\Winre.wim

after setting the path 

Windows will automatically assign the BCD, Partition ID and flags

the last step is Running 

<Code>Reagentc /enable</code>

then you would be technically done with this recovery

You just need to make sure that all of WinRE functionalities are there 

By well booting to it 

the normal options should include 

* Reset this PC 

* Command Prompt 

* Startup Repair 

* etc 


and we should be done here 
if there was some parts that was misunderstood or you are maybe stuck on some part 

[you can see this video yes it is not the best edit in the world and it is not my job but i tried](https://www.youtube.com/watch?v=e7tWWBpIsrQ)





## GPT 

* if your disk is GPT Partitioned and you have UEFI 

your life is much much Easier than the MBR guys 

you won't have the same By default identical partitions 

and this follows very easy steps exactly like the MBR procedures but a bit different 


The Recipe for Success is: 


* Open Testdisk you will navigate with the arrow keys 


* Select your Main disk that you want to recover then hit Enter 


* For MBR devices you want to select / Highlight then hit enter on [GPT EFI] 


* Select / Highlight Analyze and Hit Enter


* Then Select / Highlight Quick Search and Hit Enter and hit enter 



* Now when you want to recover the partitions use your Left and right arrow keys to select the partition type usually we make all Primary [P]

And this time we don't need to worry about partition limits 

before you go all nuts and set everything as Primary 

it will not work 

Test disk will show you some partitions that if you did set to Primary the structure will be bad 

How can you know which from which ?



* To know Which partitions from Which before we write anything Select a partition then press P on your Keyboard to list the files 

After Listing the files you will see what files Correspond to the Current Partition selected 

we need 3 Partitions selected as PRIMARY

But how about the other 2 or whatever 

IF you list a Partition files and its insides and it tells you the filesystem may be damaged 

Ignore it and leave it as D or Deleted 

if it has files then set it as Primary 

that is it No Duplicate confusion no nothing

now highlight Confirm then



* Hit Enter then navigate to [Write] Hit Enter then Hit Y 

Note: This DOESN'T WRITE YOUR DATA 

NOT DIRECTLY NOT INDIRECTLY IT ONLY WRITES SOMETHING CALLED THE PARTITION TABLE AND STRUCTURE 

NOT YOUR DATA

YOUR DATA MAY BE IN DANGER ONLY WHEN WRITING PARTITIONS THAT OVERLAP WITH OTHERS BUT THAT IS THE WORST CASE 

AND I DON'T WANT THAT TO HAPPEN 

I DON'T WANT THAT YOU DON'T WANT THAT NO ONE WANTS THAT


* Split 

Now if your Plan was to make the partitions accessible again so you can backup your data 

here we are done you do not need to proceed you can backup your data by plugging in the drive to a another computer 

then copying your valuable data 

if you want to restore Complete Bootability with Every Functionality possible including WinRE 

then Move on


* Boot again to HBCD 

after Booting back to HBCD or your USB drive that has HBCD

Now things get easier and more simpler now trust me 

what i want you to do is 

Know the Partition letter that Corresponds to which partition 

then write that down 

for example i have 4 partitions 

C:\, D:\, E:\, F:\ (Exclude the X:\ partition)

now on their own they are useless data they are just letters

but when you enter and check their files and know which from which is critical 

now access each Partition find where your Windows install is 

Say E: 

and find where the Boot Partition or system partition is 

just enter the partition and if you find bootmgfw.efi along with other files 

and the partition is About 200MB big 

before we proceed 

when you find that partition delete the file named BCD that is in it 

it is already corrupted and wont serve us any purpose

then from there we will be able to craft our command to restore the Boot Configuration data Otherwise known as the BCD 


now for example 

my Windows install is in E:\ and my System partition which has THE BOOT FILES remember boot files are different than your Main windows install for example is C:\ 

then we can type in our command 

bcdboot E:\Windows /s C: /f UEFI 

that will restore the boot config data 

But remember when you don't know the Partition letters it is more like 

Syntax:

bcdboot (OS Drive Letter)\Windows /s (System\boot partition Drive letter) /f UEFI

you must fill in the blanks tell BCDBoot where your Windows install and boot partition is help it so it can help you

that is why i told you 

Identify which partition from which


* After Successfully running the command 

Now that you did run the command and it worked 

just a last step before we boot to Windows 

go to the Windows Partition and search for the file reagentc.xml 

it is usually in 

\Windows\System32\Recovery\reagentc.xml 

when you locate it delete it 


* Restart


* After Booting and ensuring that your Environment is clear and is not Compromised if Unsure Run [Disinfect A Computer](https://github.com/Salah-Code-Lab/Raising-Awarness-Empowering-Users/blob/main/Disinfect%20a%20Computer.md)


* Open up CMD as Administrator or with HIGH_INTEGRITY_TOKEN rights 

then the path to the winre image remember it 

(Letter)\Recovery\WindowsRE\Winre.wim 

with the recovery partition letter in the beginning 

then we would need to run 

reagentc /setreimage /path (Drive Letter for the recovery partition)\Recovery\WindowsRE\Winre.wim

after setting the path 

Windows will automatically assign the BCD, Partition ID and flags

the last step is Running 

<Code>Reagentc /enable</code>

then you would be technically done with this recovery

You just need to make sure that all of WinRE functionalities are there 

By well booting to it 

the normal options should include 

* Reset this PC 

* Command Prompt 

* Startup Repair 

* etc 



## Edge Case 


**I Did not find the Winre.wim not in CMD not in Explorer and the recovery partition has nothing in it yet it has only about 150MB free**


That is a visiblity issue 

when you go to CMD and mount the partition 

if you do Dir (Drive Letter)\ /a it will list all of the files 

and the Winre.wim is there 

if it is not there 

then it should IT SHOULD be at (OS drive Letter)\Windows\System32\Recovery\Winre.wim 

then the command would be  

reagentc /setreimage /path (Drive Letter for the Main OS partition)\Windows\System32\Recovery\Winre.wim


if Both are missing at the same directories 

then back up your data and reset your PC 

because there may be something actively killing your way of recovery always

[you can see this video yes it is not the best edit in the world and it is not my job but i tried](https://www.youtube.com/watch?v=orPdDuhufIQ)







and for that manner that is why i made a demonstration video that is unscripted 

that issue did drive me crazy on how to explain it but i don't know if you did understand me 

that is why i always provide my Session ID




if you find any issues please contact me or/and raise an issue 


> 056bf8ea1a057b4f351d8b651944252cd4d88416ce6c11761f0c406f228a302301 


that is my session ID 
