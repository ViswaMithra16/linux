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
1. Worked on the CPUID leaf node %eax=0x4FFFFFFF and %eax=0x4FFFFFFE
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

Create a Inner VM inside a VM using the below commands:
1. sudo apt update
2. sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
3. sudo systemct1 status libvirtd
4. sudo systemct1 enable --now libvirtd
5. sudo apt install virt-manager
6. sudo virt-manager
7. Install ubuntu 20.4 iso image
8. Installing CPUID package
9. Download the CPUID deb package for AMD64 https://packages.ubuntu.com/bionic/admin/cpuid
10. Installed the package and execute install using sudo dpkg -i cpuid_20170122-1.deb
11. Executed the below command inside VM
        cpuid -l 0X4fffffff -s exit_number
        cpuid -l 0X4ffffffe -s exit_number
