# Create Vm Hypervsior Using PowerShell

## Create a virtual machine by using Hyper-V Manager

- Open Hyper-V Manager.
- From the Action pane, click New, and then click Virtual Machine.
- From the New Virtual Machine Wizard, click Next.
- Make the appropriate choices for your virtual machine on each of the pages. For more information, see New virtual machine options and defaults in Hyper-V Manager later in this topic.
- After verifying your choices in the Summary page, click Finish.
- In Hyper-V Manager, right-click the virtual machine and select connect.
- In the Virtual Machine Connection window, select Action > Start.

## Create a virtual machine by using Windows PowerShell
- On the Windows desktop, click the Start button and type any part of the name Windows PowerShell.
- Right-click Windows PowerShell and select Run as administrator.
- Get the name of the virtual switch that you want the virtual machine to use by using Get-VMSwitch. For example,
```
Get-VMSwitch  * | Format-Table Name
```

Use the New-VM cmdlet to create the virtual machine. See the following examples.
Existing virtual hard disk - To create a virtual machine with an existing virtual hard disk, you can use the following command where,

- Name is the name that you provide for the virtual machine that you're creating.
- MemoryStartupBytes is the amount of memory that is available to the virtual machine at start up.
- BootDevice is the device that the virtual machine boots to when it starts like the network adapter (NetworkAdapter) or virtual hard disk (VHD).
- VHDPath is the path to the virtual machine disk that you want to use.
- Path is the path to store the virtual machine configuration files.
- Generation is the virtual machine generation. Use generation 1 for VHD and generation 2 for VHDX. See Should I create a generation 1 or 2 virtual machine in Hyper-V?.
- Switch is the name of the virtual switch that you want the virtual machine to use to connect to other virtual machines or the network. See Create a virtual switch for Hyper-V virtual machines.
```
New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>
```

For example:
```
New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch
```
This creates a generation 2 virtual machine named Win10VM with 4GB of memory. It boots from the folder VMs\Win10.vhdx in the current directory and uses the virtual switch named ExternalSwitch. The virtual machine configuration files are stored in the folder VMData.

New virtual hard disk - To create a virtual machine with a new virtual hard disk, replace the -VHDPath parameter from the example above with -NewVHDPath and add the -NewVHDSizeBytes parameter. For example,

```
New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch
```
New virtual hard disk that boots to operating system image - To create a virtual machine with a new virtual disk that boots to an operating system image, see the PowerShell example in Create virtual machine walkthrough for Hyper-V on Windows 10.

Start the virtual machine by using the Start-VM cmdlet. Run the following cmdlet where Name is the name of the virtual machine you created.
```
Start-VM -Name <Name>
```
For example:
```
Start-VM -Name Win10VM
```

Connect to the virtual machine by using Virtual Machine Connection (VMConnect).
```
VMConnect.exe
```
