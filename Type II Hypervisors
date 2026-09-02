## Type II Hypervisors

> Note: This is focused On VMWare and assumes the setup is for testing or analysis.

---

## Definition

Type II hypervisor is a Software that runs on a Host Machine that has a Operating system installed that Simulates a Virtual non Existent machine and executes Instructions and provides resources that the OS will need for Execution.

---

## Fundamentals

A Type II Hypervisor not to be Confused with Type I Hypervisors is a Hypervisor that creates a Virtual Machine (VM) that simulates like an Emulator.

Type II Hypervisors can:

- Create, Manage, delete the Virtual Machine
- Create Snapshots or Saved states of an Operating System at the time of the Saved state was being taken
- Virtualizes Many Vectors to make the Host OS unaffected to what happens inside the Virtual Machine

Though it completely Isolates the Virtual Machine from the Host OS, it still can be Escaped.

It still can be Manipulated to Consume Resources from the Host to For example Mine Crypto.

It still Exposes Vectors that Can make attackers or malicious software Identify the machine and the Hypervisor company (VMWare: BroadCom | Hyper-V: Microsoft | VirtualBox: Oracle, etc etc).

And from the Identifications It can behave Differently across Different hypervisors.

Take Roblox for example:

On VirtualBox it would cause the OS to freeze which causes:
- Destruction of Volatile Memory
- Intentional DoS
- Etc.

But in VMWare it would just exist with a Critical error (which is faked).

Sadly that is how Software is these Damn days.

---

## Setup

Since you know we are Using VMWare you can download it from here:
[VMWare](https://www.techpowerup.com/download/vmware-workstation-pro/)

Install it and you will be Presented with a GUI interface that will bombard you with options.

But they will all make sense in a Moment.

Basically we need to create a New Virtual Machine so in VMWare press:

`Ctrl+N`

Or go to:
`File` in the top right corner and click on `Create new Virtual Machine`.

- Select `Custom Setup`
- Workstation (Latest Version)
- Then hit next

Select `Installer Disc Image or Iso`.

Then Browse until you find your desired Iso Assuming that it is Win11.

- Select your Iso then name the VM name to whatever what you want.

Then it will Prompt you in a Encryption Notification if you are going to move the VM from place to place.

To not forget just type in `12345678` you don't really need the Encryption for a Analysis or Test VM.

- For Win11 use `UEFI` and **Do not select Secure boot**.
> Secure Boot blocks unsigned/test-signed drivers, which we need for analysis driver.

- Now the tricky part:

**Processor Configuration**

Make sure to Explicitly set:

- Number of Processors: `1`
- Number of cores Per Processor: `2`

This is **CRITICAL** we want two cores for one processor.

**AND NOT** two processors with each one having one core.

Then Select the Desired RAM size. My Opinion? If you have a 32GB RAM use `8192`. If you only have 16GB use `4096` not a Problem.

- **Network Type:** Select `NAT` the most secure if something wrong happens and is actually good unlike Bridged networking.

- **SCSI Controller:** just use what VMWare recommends.

- **Disk Type:**
  - If your SSD is a NVME use `NVME`
  - If your SSD is SATA use `SATA`
  - If you are on a HDD use `SCSI` or `IDE`
  - I recommend `SCSI` if you have a HDD.

Then create a new Virtual Disk.

Specify only `60GB` of Storage and do not `Allocate all disk space now`.

The `Store Virtual disk as a single file` is recommended by me its easier for moving the file more efficiently and wont need any edits if you expand the disk later.

- Name the VMDK to your Needs.

- After all of that make sure of your selected options for directory install, etc etc.

**Note:** If you chose a NVME controller but put the VMDK of the actual VMWare on a HDD drive you will get HDD level Speeds not SSD level speeds which is pain.

Go ahead and install the Virtual machine then Comeback to me.

---

## Configure the VM

After installing the VM if you want to exit and no longer give Input to the VM press `ctrl + alt` together which will take input to the host and not the VM.

In the top right corner click on `VM` then `Install VMWare tools`.

Then go to the VM again or click in it.

Use `Win + E` or open Explorer normally.

Click on the Partition that shows or has an icon of vm.

And the Install should Trigger.

After the install triggers click on continue until you are prompted to choose install type.

Choose `Full` we don't want anything missing later.

After installing VMWare tools, Update Windows.

Finish updating them comeback to me.

Now that you installed Windows Latest updates, Shut down the VM and select your VM after shutting down and go to settings.

From there i want you to go to `Network Adapter` select it with clicking on it.

Then Remove it OR disable it by disabling `Connected` and `connected at Power on`.

Then boot to the VM again log in and from there.

**Take a Snapshot.**

---

## Snapshots

Snapshots **ARE A LIFELINE** of any analyst.

It literally saves you HOURS.

What it does is basically save the OS current state.

The OS is unbootable or Untrusted? you lost data?

Easy: restore the Last saved OS state and everything is back to normal and clean.

How to snapshot?

Basically you see the three Clock Icons?

[I should include a image of them here]

- The first on the left takes the snapshot.
- The one in the middle restores to the last saved snapshot that was taken.
- The third one allows you to manage that snapshot(s) that you did take (for example you wanted to delete one, you'd manage it from the third one).

And basically that is all what you need to know the absolute basics.

The Getting Comfortable with the tool and making it internalized is On you not on me.

So i will leave the rest for you to Explore literally experiment with VMWare it is a interesting piece of software and the VMs are even more interesting.
