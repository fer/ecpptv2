---
description: Understanding of x86 Architecture and its weaknesses.
---

# TBD - System Security

{% hint style="danger" %}
**This document is still in progress...**&#x20;
{% endhint %}

## Architecture Fundamentals

{% hint style="info" %}
**Goal:**

Improve skills and provide a solid foundation in fuzzing, exploit, development, buffer overflows, debugging, reverse engineering and malware analysis.

**Keywords:**

x86/x64, assembly, compilers, ASLR, DEP, BufferOverflow.
{% endhint %}

### Architecture Fundamentals - Study Guide



### :arrow\_forward: Stack Frames

### Security Implementations - Study Guide

### :test\_tube: System Security

## Assembler Debuggers and Tool Arsenal

### Introduction - Study Guide

### Assembler - Study Guide

### Compiler - Study Guide

### NASM - Study Guide

### Tools Arsenal - Study Guide

### :arrow\_forward: Immunity Debugger

## Buffer Overflows

### Understanding Buffer Overflows - Study Guide

### :arrow\_forward: Debugging Buffer Overflows Goodpassword

### Finding Buffer Overflows - Study Guide

### Exploiting Buffer Overflows - Study Guide

### Exploiting a Real World Buffer Overflow - Study Guide

### :arrow\_forward: Exploiting Buffer Overflows 32bit FTP

### Security Implementations - Study Guide

## Shellcoding

### Execution of a Shellcode - Study Guide

### Types of Shellcode - Study Guide

### Encoding of Shellcode - Study Guide

### Debugging a Shellcode - Study Guide

### Creating Our First Shellcode - Study Guide

### A More Advanced Shellcode - Study Guide

### Shellcode and Payload Generators - Study Guide

## Cryptography and Password Cracking

### Introduction - Study Guide

### Classification - Study Guide

### Cryptographic Hash Function - Study Guide

### Public Key Infrastructure - Study Guide

### Pretty Good Privacy (PGP) - Study Guide

### Secure Shell (SSH) - Study Guide

### Cryptographic Attacks - Study Guide

### Security Pitfalls Implementing Cryptographic Systems - Study Guide

### Windows Passwords - Study Guide

## Malware

### Classification

> Malware: Malicious Software

Malware is basically a software written to cause damage or infiltrate computer systems without the owner's informed consent. It is a general term used to represent various froms of intrusive, hostile and/or annoying code.

* Virus
* Trojan Horse
* Rootkit
* Bootkit
* Backdoor
* Adware
* Spyware
* Greyware
* Dialer
* Key-logger
* Botnet
* Ransomware
* Data-stealing Malware
* Worm

#### Virus

A computer virus is a computer program that copies itself and spreads without the permission or knowledge of the owner. Viruses _do not spread via exploiting vulnerabilities_ (the ones that do that are called _worms_).

The only way viruses are supposed to spread is with the host, at least in their rigorous classification. Let us say, that a virus has infected a file; now if the owner moves the file to any system, the virus has thus a chance to spread and survive.

Types:

* **Resident type:** when executed becomes memory resident (and waits for some triggers such as loading of other program). It then infects other programs and so on.
* **Non-resident type:** once a virus is executed, it will search for files it can infect. Then after infecting them, it will quit. When the infected program is executed again, it will again find new targets and so on.
* **Boot-sector virus:** which spreads via boot sectors. For example, if a user leaves a infected CD-ROM while turning off a system, the next time system will boot-up, the boot sector virus will activate and will thus spread to the hard-disk which will then spread it to another floppy disks. When floppies/pendrives are removed, the cycle gets repeated.
* **Multi-partite type:** These viruses have several types of infection mechanisms such as they can have both Boot-sector and resident type virus or even more.

#### Trojan horse

Kind of malware that appears to the user to perform a function, but in fact facilitates unauthorized access to the owner's system. They don't self-replicate unlike viruses.

#### Rootkit

Malware which is designed to hide the fact that a compromise has already been done or to do the compromise at a deeper level. It's normally used as a supplement to other malware. They can be used to hide processes, files on the file system, implement backdoors and/or create loopholes.

Rootkits exist for all major operating systems such as Windows, Linux, Solaris, OS X, etc.&#x20;

Basically installed as drivers (or kernel modules). They are known to exist as the following levels (even lower levels are possibly):

* **Application level**: they replace actual programs with copies of other programs.
* **Library level**: let us say that 10 applications are sharing a library, taking control of the library means taking control of all 10 apps.
* **Kernel level**: this is the most common type and was first developed by Greg Hoglund around 1999 for Windows NT. They are known for their resistance to removal since they run at the same privilege level at which Anti-virus solutions run.
* **Hypervisor level**: these days, processors have come up with support for virtualization. Rootkits which use such processor specific technologies are called _hyper-visor rootkits_ (`blue-pill` and `subvirt`).
* **Firmware level**: Rootkits for firmware such as BIOS, ACPI tables or device ROMS are known to exist. They have the highest chance of survival because currently, no tools exist to verify/scan up the firmware level rootkits.

#### Bootkit

Bootkits are rootkits which grab the OS during the boot process itself and were introduced by Nithin Kumar and Vipin Kumar in 2007 (authors of this section).

They differ from the rootkits in the installation process and how and when they take control of the OS.&#x20;

They start attacking the OS when the OS has not even started, so they are able to completely violate the security of the target OS.&#x20;

#### Backdoor

Software (or modification to the software) which helps in bypassing authentication mechanism, keeping remote access open (for later unauthorized purpose) which trying to remain hidden.

For example, a backdoor in a login system might give you access when a specified username/password is entered, even though they might not be a valid combination.

#### Adware

Basically advertising supported software which displays ads from a time-to-time during the use of the software.&#x20;

Some adware also act as spyware. Adware also install other unwanted software on the users system which might/might not be malware without owner's consent

#### Spyware

Kind of malware which keeps on spying the user activities such as collecting user information (what he types), his website visiting record and other information without the consent of the computer owner.&#x20;

The information is sent to the author after a certain about has been collected.

They are also called privacy-violating software or privacy-invading software.

Normally a system which has spyware also has other kinds of malware such as rootkits/trojans to hide the tracks and to keep in control of the machine.

#### Greyware

This is a collective name of spyware and adware. A greyware can be either a spyware or adware or both. Thus, all spyware and adware software are collectively referred as greyware.&#x20;

#### Dialer

Software used to connect to the internet but instead of connecting to normal numbers, they connect to premium numbers which are charged highly.&#x20;

Thus, the owner of the dialer who has setup the stuff makes big sums of money.&#x20;

#### Key-logger

Keyloggers are malware which log down key pressed by the key-owner without the consent of the owner. Thus, the person is unaware that his actions are being monitored.&#x20;

For example, a person might type his credit-card numbers which might then be misused by the creator of keylogger.

Types:

* Software keylogger: kernel mode or user mode keyloggers.
* Hardware keylogger: firmware based keylogger can be put in the BIOS.
* Wireless keyboard sniffer: PS/2 and USB keyboards can be sniffed with an additional device placed between the keyboard port and CPU. Passive sniffers can be used to collect keyboard data in case of wireless keyboards.
* Acoustic keylogger: these kind of keyloggers are based on the sound made when a key is struck by the user. After some time of data logging, clear patterns can be distinguished when a key is pressed or release which leads to remote passive keylogging.
* Optical keylogger: optical keylogging can be done by a person standing beside you. This technique is also called shoulder surfing and is normally used to steal ATM PINs or passwords or other small but critical pieces of information.&#x20;

#### Botnet

It refers to a collection of compromised computers which run commands automatically and autonomously (with the help of command and control server).&#x20;

Botnets are typically created when a number of clients install the same malware.&#x20;

This is usually done via drive-by-downloads (compromised website will try to exploit your web browser and install a software without user consent).

The controller or owner of the botnet is called a bot master and is usually the one who gives commands to the bots.&#x20;

Botnets are used by the botmaster for reasons such as distributed DoS, sending SPAM, etc.

#### Ransomware

Software that locks down important files with a password and then demands from the user to send money and in return promises to unlock files.

They are also called extortive malware since they demand extortion money (ransom) for restoration of user data.

The most famous example being `gpcode` which used public-key cryptography to encrypt the user files.

#### Data-Stealing malware

It basically steals data such as private encryption keys, credit-card data, competitors data such as internal secret algorithms, new product designs and other internal data which could be used by a third party to cause damage to the original data owner.&#x20;

Some of these are highly targeted attacks and are never detected.

#### Worm

Software which use network/system vulnerabilities to spread themselves from system to system.

They are typically part of other software such as rootkit and are normally the entry point into the system.&#x20;

They basically compromise the system (locally or remotely) and then provide access to other software such as bot clients, spyware, key-loggers and so on.

### Techniques Used by Malware

The goal is to understand the threat level and be able to provide solutions accordingly. This sort of techniques are basically used by malware to hide themselves from either the anti-virus, the user or both.

The most important methods are:

* Streams
* Hooking native APIs / SSDT
* Hooking IRP

#### Streams

Streams are a feature of NTFS file system, they are not available on FAT file systems. Microsoft calls them Alternate Data Stream.

The original data stream is file data itself (it is the data stream with no name), all other streams have a name. Alternate data streams can be used to store file meta data/or any other data.

```
echo this data is hidden in the stream >> sample.txt:hstream
more < sample.txt:hstream
```

In the `CreateFile` API in Windows, just append `:stream_name` to the filename, where `stream_name` is the name of the stream.

#### Hooking native APIs / SSDT (System Service Descriptor Table)

Native API is API which resides in `ntdll.dll` and is basically used to communicate with kernel mode. This communication happens using SSDT table.

For each entry in SSDT table, there is a suitable function in kernel model which completes the task specified by the API.

SSDT table resides in the kernel and is exported as `KeServiceDescriptorTable`. The following are the services available for reading/writing files:

* NtOpenFile
* NtCreateFile
* NtReadFile
* NtWriteFile
* NtQueryDirectoryFile (this is used to query contents of the directory)

Microsoft keeps on adding new services on every OS release.

Let us consider the case of a directory query. for that, we have to hook `NtQueryDirectoryFile`. Hooking means that we want our (malicious) function to be called instead of the actual function:

1. Hook SSDT table entry corresponding to `NtQueryDirectoryFile.`
2. Now, whenever the above function is called, your function will be called.
3. Right after your function gets called, call original function and get its result (directory listing).
4. If the result was successful, modify the results (hide the file/sub-directory you want to hide).
5. Now pass back the results to the caller.
6. You are hidden.

This is a basic method. Nowadays almost all anti-virus/rootkit-detectors scan SSDT table for modifications (they can compare it with the copy stored in the kernel) and thus detection can be done.

#### Hooking IRP

Windows architecture in kernel mode introduced the concepts of IRPs (I/O request packets) to transmit piece of data from one component to another.

The concept of IRPs is well explained in the Windows Driver Development Kit (it is available for free).

Almost everything in the windows kernel use IRPs for example network interface (TCP/UDP, etc), file system, keyboard and mouse, and almost all existent drivers.

```
DriverObject->MajorFunction[IRP_MJ_CREATE] = DiskPerfCreate;
DriverObject->MajorFunction[IRP_MJ_READ] = DiskPerfReadWrite;
DriverObject->MajorFunction[IRP_MJ_WRITE] = DiskPerfReadWrite; 
DriverObject->MajorFunction[IRP_MJ_SYSTEM_CONTROL] = DiskPerfWmi;
DriverObject->MajorFunction[IRP_MJ_SHUTDOWN] = DiskPerfShutdownFlush;
DriverObject->MajorFunction[IRP_MJ_FLUSH_BUFFERS] = DiskPerfShutdownFlush;
DriverObject->MajorFunction[IRP_MJ_PNP] = DiskPerfDispatchPnp;
DriverObject->MajorFunction[IRP_MJ_POWER] = DiskPerfDispatchPower;
```

There are basically 2 ways to play with IRPs:

* **Become a filter driver**: register with the OS as a filter driver or an attached device.
* **Hooking the function pointer**: the array is just a table with function pointers and can be easily modified.

Code snippet showing function pointer hooking:

```
old_power_irp = DriverObject->MajorFunction[IRP_MJ_POWER];
DriverObject->MajorFunction[IRP_MJ_POWER] = my_new_irp;
```

Function pointer is one of the easiest methods to hook functions. The basic IRP design is so that after an IRP has been created, it's passed to all the devices registered at lower levels. The design has pre-processing mode and post-processing mode.

Pre-processing is done when an IRP arrives, and post-processing is done when the IRP has been processed by all the levels below current level.

Each device object has its own function table. Hooking the function pointers of such objects is called DKOM (Direct Kernel Object Manipulation). All file-systems, network layers, devices like keyboard, mouse, etc., have such objects.

For example:

* `\device\ip`
* `\device\tcp`
* `\Device\KeyboardClass0`
* `\FileSystem\ntfs`

Filter drivers are basically used by anti-viruses to get control whenever a new file is written.&#x20;

#### Hiding a process

Hiding a process requires a more difficult approach.&#x20;

It requires a combination of different techniques.

E.g., first thing you have to do is to hook `NtOpenProcess` native API (probably using SSDT table hooks).&#x20;

Something else we could do is to hide the process from `EPROCESS` list. This list is maintained by the OS for all the active processes. The easiest thing to do is to unlink the structure relative to our process from the list.&#x20;

If the driver is loaded, you will also have to unlink it from the `PsLoadedModuleList`.&#x20;

API hooking is essentially the act of intercepting an API function call and modifying its functionality somehow, either by redirecting it to a function of our choice, stopping the function from being called or logging the request, the possibilities are infinite.

There can be different types of hooking such as:

* **IAT Hooking**: Import Address Table. It is basically used to resolve runtime dependencies (when you use `MessageBoxA` API in windows, your compiler automatically links to `user32.dll`). IAT hooking involves modifying the IAT table of the executable and replace the function with our own copy.
* **EAT Hooking**: Export Address Table. This table is maintained in DLLs (dynamic link library). These files just contain support functions for other executable files.
* **Inline Hooking**: most difficult due to the way it works. In this form of hooking, we modify the first few bytes of the target function code and replace them with our code which tells the IP (instruction pointer) to execute code somewhere else in memory. Whenever the function gets executed, we will get control of execution; after doing our job, we have to call the original function so we have to fix up the modified function. This is normally by executing a number of instructions which were replaced and then resuming execution in non-modified original function code.

The difference between **IAT** and **EAT** hooking is since EATs exist only in DLL files (under normal settings) most of the times EAT hooking is utilized only on DLLs while IAT hooking can be done on both EXEs and DLLs.

#### Anti-debugging methods

There are several methods which are used by malware to increase the time required to analyze the code \*by security analysts). If such techniques are not already known by security analysts, then the time required increases drastically.

**INT 2D trick** (for Windows OS)

```
push debugger_not_detected
push fs:[0] // set SEH
mov fs:[0],esp
int 2dh // if debugger is attached, it will run normally else we have got an exception
nop 
pop fs:[0]
add esp, 4
...
debugger_detected:
...
debugger_not_detected
```

What we do is:

1. Set an exception handler
2. Cause an exception with `INT 2dh`
3. If a debugger is attached and does not pass the exception to us, we get to `debug_detect` because an exception ocurred for sure (we caused it).

#### Anti-Virtual Machine

Normal users always run the programs on a real system but security analyst analyzing malwares do not run the malware on a real system.

They always run the code in virtualized OS.

The tools are VMware, Virtual PC, Xen, bohs, qemu to name a few.

These software let you install a virtual OS side-by-side your OS without disturbing your OS and will run just like any other normal program.&#x20;

These techniques are basically used by security analysts, so malware authors have found out few bugs in these applications which can be used to detect whether the OS is virtualized or not.

The techniques basically work on the SIDT instruction, which returns the IDT table address.&#x20;

On real machines, it is on low memory less than 0xd0 while for virtualized OS (VMWare), it is higher than that. This abnormal behavior leads to detection whether the malware is running on a real or virtualized system or not.

#### Obfuscation

Code obfuscation techniques transform/change a program in order to make it more difficult to analyze while preserving functionality.

It is used both by malware and legal software to protect itself.

The difference is that malware use it to either prevent detection or make reverse engineering more difficult.

Basically code obfuscation/data obfuscation makes programs more difficult to reverse engineer. One major drawback of existing obfuscating techniques is the lack of theoretical basis about their efficiency (several implementations which looked impressive had very basic weak points, leading to their total downfall).&#x20;

Th malware obfuscates itself every time it infects a new machine making it harder for a detector to recognize it.

Existing malware detectors (antivirus engines) are based on signature matching, thus they are based on purely syntactic information and can be fooled by such techniques.

#### Packers

Packers are software which compress the executable. They were initially designed to decrease the size of executable files. However, the malware authors recognized very quickly that decreasing file size will decrease number of patterns in the file, so less chances of detection by antivirus. Antivirus basically work by matching patterns (signatures). So this effectively increases the chances of malware to go undetected.&#x20;

Some virus writers have gone to the limit of creating their own packers (suck as Yoda packer) while others use readily available packer such as UPX.&#x20;

Some packers use anti-debugging tricks also.

Packer facts:

* Packers allow to compress/encrypt applications.
* You cannot see the code of the application using a disassembler, you need to unpack it first.
* Packers compress applications and add a small loader to the file.
* The loader will decompress the binary in memory, resolve imports, and call the Original Entry Point (OEP).

#### Polymorphism

Polymorphic code aims at performing a given action (or algorithm) through code that mutates and changes every time the action has to be taken.&#x20;

The mutation makes them very difficult to detect.

There have been only a few polymorphic viruses and they still are not detected 100% by most of the anti-viruses.&#x20;

All polymorphic viruses have a constant encoding and variable decryptor. So a virus using a different XOR key to encrypt its variant also falls into polymorphic category.

#### Metamorphism

IT can be best defined as polymorphism with polymorphism applied to the decryptor/header as well.&#x20;

There are numerous ways to implement metamorphism/polymorphism (both are similar with some minor differences). Some of which are documented below:

* **Garbage insertion**: garbage data/instructions are inserted into the code, for example NOP instructions (0x90) are inserted.
* **Register exchange**: the registers are exchanged in all the instructions.&#x20;
* **Permutation of code blocks**: code blocks are randomly shuffled and then fixed up, so that the execution logic is still the same.
* **Insertion of jump instructions**: some malware mutate by inserting jumps after instructions (the instruction is also relocated), so that the code flow does not change.
* **Instruction substitution**: one instruction (or set of instructions) are replaced by 1 or more different instructions which are functionally equivalent to the replaced set.
* **Code integration with host**: malware modifies the target executable (which is being infected) by spraying its code in regions of the EXE. `Zmist` virus used this technique very effectively. To hide more changes such as changes in the file size, the malware might compress the original code to survive.







### How Malware Spreads - Study Guide

### Samples - Study Guide
