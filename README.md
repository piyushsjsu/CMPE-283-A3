<h2 align="center">CMPE-283 Assignment 3 (Instrumentation via hypercall)</h2>

<b>Please refer screenshots folder to verify the completion of this assignment.</b>
<br/>
<br/>

<h2>Questions</h2>

   * <h5>Does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations?</h5>
   
      * Some exits do increase at a stable rate while some of them stay the same. When a browser is started or a process is executed some exits jump at an unstable rate.
   
   * <h5>Approximately how many exits does a full VM boot entail?</h5>
   
      * The VM boot entails approximately 1284502 exits with total cyle time : 18733836564.
   
   * <h5>Of the exit types defined in the SDM, which are the most frequent? Least?</h5>
   
      * Of the exit types defined in the SDM, EXIT_REASON_EPT_VIOLATION(48) occurs most frequently (please refer screenshots for proof) and EXIT_REASON_DR_ACCESS(29) occurs least frequently.   

<br/>

<h4>Work done by each member of the team.</h4>

   * Piyush Mamidwar (016709966)
      * Modified the vmx.c file to implement leafs 0x4FFFFFFC, 0x4FFFFFFD, 0x4FFFFFFF and 0x4FFFFFFE.
      * Wrote test.c file to print total exits and total cycles for all types of exits.
      * Debugged errors that occured after editing vmx.c and cpuid.c files.
      * Install virt manager and the nested VM and installed ubuntu OS on it.
      * Figured out how to access inner VM on GCP. Couldn't access inner VM using CLI. (Used Chrome remote desktop)
      * Wrote steps on how to perform this assignment.
  
   * Harsh Patel (016567005)
      * Configured the VM to enable nested virtualization and to make builds faster and increased storage space.
      * Cloned the linux repo and configured it correctly for the current VM installed all the required modules to build it.
      * Modified the vmx.c file to count total number of exits and to find total cycles for all types of exits.
      * Built and installed the modified kernel and verified if it is correctly installed.
      * Wrote some of the steps on how to perform this assignment.
<br/>
<h4>Below are the steps for completing the assignment using GCP.</h4>

1. Create a VM Instance on GCP having atleast 8 cores and 32GB RAM (To make the builds faster) with nested virtualization enabled.

2. Run the below commands to install the required modules:
   * ```sudo apt-get install gcc```
   * ```sudo apt-get install make```
   * ```sudo apt-get install flex```
   * ```sudo apt-get install bison```
   * ```sudo apt-get install libssl-dev```
   * ```sudo apt-get install libelf-dev```
   * ```sudo apt-get install build-essential```
   * ```sudo apt-get install kernel-package```
   * ```sudo apt-get install ccache```
   * ```sudo apt-get install flex```
   
3. Fork (https://www.github.com/torvalds/linux) repo.

4. Clone the forked repo in your VM using the following command:
   * ```git clone https://github.com/<your_username>/linux.git```
   
5. Go to the cloned linux folder and copy the config of your VM's kernel to the current directory. Use the below command:
   * ```cp /boot/config-5.15.0-1025-gcp ~/linux/.config```

6. Build the kernel from the config we just copied. Run the following commands:
   * Build the kernel using the already existing config file
      * ```make oldconfig```
   * Prepare the linux
      * ```make prepare```
   * Build the modules
      * ```make -j <num_of_available_cores> modules```
   * Build the kernel itself
      * ```make -j <num_of_available_cores>```
   * Packaging the modules
      * ```sudo make INSTALL_MOD_STRIP=1 modules_install```
   * Install full built kernel to the VM
      * ```sudo make -j <your-vm-cpu-total-core-number> install``` 
   * Reboot to see changes
      * ```sudo reboot```
      
7. Run the below command to check if our kernel is installed:
   * ```uname -a```

8. To make changes in the kvm hypervisor, navigate to 'arch' directory:    
   * ```cd ~/linux/arch/x86/kvm```
 
9. Replace or modify vmx.c and cpuid.c files with the files present in this repo.

10. Go back to the top level of linux and perform Step 6.

11. Create an VM inside our current VM (nested VM). Install  virtual manager first using the below command:
    * ```sudo apt-get install virt-manager```
    
12. I tried to interact with the inner VM using the CLI but failed. I ended up using chrome desktop server.
    * https://cloud.google.com/architecture/chrome-desktop-remote-on-compute-engine
    
13. Go to https://remotedesktop.google.com/headless and follow the steps to connect to your inner VM instance.

14. Download ubuntu image using the following command:
    * ```wget https://releases.ubuntu.com/jammy/ubuntu-22.04.1-desktop-amd64.iso```

15. Open virt manager and install this ubuntu image on your inner VM using the GUI.
    * ```sudo virt-manager```
 
16. Now we can perform Assignment 3 on the inner VM. Create or copy test.c file from this repo on to you inner VM.

17. Compile and run test.c to verify the result.
   

