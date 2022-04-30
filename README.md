# Assignment-1

Name : Viswamithra Vallabhaneni.   
Student Id: 015570516.

## Steps to run the Assignment-1

1. Start your VMWare workstation.
2. Create a VM and customize the configuration for it, enable the nested hypervisor for the VM, and 8GB of RAM and 4 CPU units.
3. I used Ubuntu LTS version as OS in the VM.
4. Once the OS is installed and and ready to use, go to https://github.com/torvalds/linux and fork the repo to our account.
5. Now go to terminal and check if our machine has the nested virtualization enabled by executing the command cat /proc/cpuinfo
6. Create a new folder 283 and perform the git clone by running the following commands
    1. git clone “repoURL”
    2. git status
7. Once the repo is cloned, copy the .c and makefile.
8. Now execute the make command  to run the MakeFile.
9. You might end up with license and version based errors. Go to clones linux directory and run cp /boot/config-5. to get the versions of kernel
10. Check the kernel version config by executing the command name -a
    1. It is config-5.13.0-39-generic 
11. Execute the command make menuconfig. Here, there are few packages that needs to be installed
    1.  Sudo apt-get install make
    2. Sudo apt-get install flex
    3. Sudo apt-get install bison
    4. Sudo apt-get install libncurses-dev
    5. Sudo apt install gcc
    6. Sudo apt install dwarves
12. Now in order to run some make commands, we need to create a .config file having contents of the specific kernel version config file.
13. Execute cp /boot/config-5.13.0-39-generic .config inside linux directory
14. Now go to .config file and comment out CONFIG_SYSTEM_TRUSTED_KEYS and CONFIG_SYSTEM_TRUSTED_KEYRING and change the value of CONFIG_SYSTEM_REVOCATION_KEYS to ""
15. Execute make oldconfig. Proceed with all the default values.
16. Now execute make prepare. Here, few packages needs to be installed.
    1. Sudo apt-get install libssl-dev
17. Run make -j x modules where x is number of cpu's that you have ( 4 in my case)
18. Run make -j 8. This builds the real kernel
19. Execute sudo make INSTALL_MOD_STRIP=1 mdoules_install
20. Execute sudo make install
21. Now go to cmp283-1.c file and add MODULE_LICENSE("GPL V2"); to get ride of the licensing error
22. Finally, run sudo reboot to let the OS select the most recent version of kernel module which is 5.18 in my case and earlier it was 5.13.0-39-generic.
23. Now run make to get the .ko kernel object.
24. To load this file execute sudo insmod cmpe 283-1.ko
25. To remove the kernel object execute sudo rmmod cmpe283-1.ko
26. If you want to make any changes to the kernel module, you can simply remove it, make the changes, make the module and simply re-install it instead of rebooting the system.
27. Commit the make file and .c file to git using the git commands
    1. git commit -m “commit message”
    2. git push

# Assignment-2

Viswamithra - %eax=0x4FFFFFFE
Sravani - %eax=0x4FFFFFFF

###### Let’s start with the first instruction 0x4FFFFFFF. Here we need to know the total exits of all types when the exit equals to 0x4FFFFFFF instruction. We can do this by incrementing it by 1 whenever the instructions equal the value, using an if condition. 

The assignment files are committed to path linux/cmpe283/Assignment -2/
1. Worked on the CPUID leaf node %eax=0x4FFFFFFF and %eax=0x4FFFFFFE both as we need to proceed further for other assignments.
2. To get the total number of exits modified the code in cpuid.c file.
3. Install the nested virtual machine inside the virtual machine
4. Now execute %eax=0x4FFFFFFF in the eax and this will return the total number of exits.


Below are the commands executed:
1. Modified the code in cpuid.c and vmx file.
2. make modules
3. make -j 4 modules
4. sudo bash
5. sudo make INSTALL_MOD_STRIP=1 modules_install && make install
6. lsmod | grep kvm
7. sudo rmmod kvm_intel
8. sudo rmmod kvm
9. modprobe kvm
10. modprobe kvm_intel

Below are some reference images for Assignment 2 and 3:

![Assigment-2 new](https://user-images.githubusercontent.com/88958925/166080390-4ec4ae7a-25c5-47fd-85fa-e091aec724b0.png)

Installing Nested VM.

![Assignment-3_2](https://user-images.githubusercontent.com/88958925/166080474-dda0d5fd-40bb-4462-addf-37c5a9abf821.png)

![fe_2_new](https://user-images.githubusercontent.com/88958925/166087632-d1a4da36-8395-44f7-9c92-70490c91f405.PNG)


Create a Inner VM inside a VM using the below commands:
1. sudo apt update
2. sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
3. sudo systemct1 status libvirtd
4. sudo systemct1 enable --now libvirtd
5. sudo apt install virt-manager
6. sudo virt-manager
7. Install ubuntu 20.4 iso image
8. Download the CPUID deb package for AMD64.
9. Installed the package and execute install using sudo dpkg -i cpuid_20170122-1.deb
10. Executed the below command inside VM
        cpuid -l 0X4fffffff -s exit_number
        cpuid -l 0X4ffffffe -s exit_number
        
        
 ## Assignment - 3
 
 Viswamithra - %eax=0x4FFFFFFD.
 Sravani - %eax=0x4FFFFFFC.
 
 
 The assignment files are committed to path linux/cmpe283/Assignment -3/
 
1. Worked on the CPUID leaf node %eax=0x4FFFFFFC and %eax=0x4FFFFFFD.
2. Modified the code in vmx.c, cpuid.c to get the total time spent in processing and to get the 32 bits of exit processing in ecx and ebx.
3. Install the nested virtual machine inside the virtual machine
4. Now execute %eax=0x4FFFFFFC in the eax and this will return the total number of exits.
5. Executed the commands with exit_number in the inner VM which is inside a VM.
        cpuid -l 0X4ffffffc -s exit_number
        cpuid -l 0X4ffffffd -s exit_number
        

 
 ## Questions
3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there 
more exits performed during certain VM operations? Approximately how many exits does a full VM 
boot entail?

Ans :- The frequency of exits is increasing at a stable rate. We have observed that VM boot for all the exits require up to ~6800000.


4.Of the exit types defined in the SDM, which are the most frequent? Least?
Most Frequent Exits:
   1. Exit number 10- CPUID
   2. Exit number 32 - WRMSR
   3. Exit number 1- External Interrupt
   4. Exit number 12- HLT
Least Frequent Exits :
   1. Exit number 54 - WBINVD or WBNOINVD
   2. Exit number 55 -  XSETBV
   3. Exit number 29 - MOV DR

## Assignment -4 

1. Execute the cpuid to get the number of exits for shadow paging.
2. Boot the guest VM and, Record total exit count information.
3. Shutdown the inner VM.
4. Remove the ‘kvm-intel’ module from current kernel.
5. Reload the kvm-intel module by giving ept=0 at the path.
6. insmod /lib/modules/5.15.0+/kernel/arch/x86/kvm/kvm-intel.ko ept=0
7. Reboot the VM, make a note of exits.


Q. What did you learn from the count of exits? Was the count what you expected? If not, why not?

When compared to nested paging, the number of exits in shadow paging increases. Nested paging eliminates the overheads associated with shadow paging. The hypervisor does not need to intercept and reproduce the visitor's modification of the guest page table, unlike shadow paging.



Q. What changed between the two runs (ept vs no-ept)?
The changes between the two runs with ept and without ept are the exit counts. The exit count has been increased with no-ept due to the overhead associated with shadow paging.
