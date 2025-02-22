# Assignment 7 - Virtualization

1. Linux is used to display detailed information about the CPU(s) on the system.
    - cat /proc/cpuinfo

    - it provides various details such as the processor type, model, number of cores, CPU speed, cache size, and more.

    ![](/img/Assignment7/1.PNG)

    ![](/img/Assignment7/2.PNG)

2. Am I using 64 bit CPU/system [x86_64/AMD64/Intel64]?
    -  If see ***lm*** flag means you’ve 64 bit Intel or AMD cpu.

    ![](/img/Assignment7/3.PNG)

3. Do I have hardware virtualization support?

    - vmx – Intel VT-x, virtualization support enabled in BIOS.
    - svm – AMD SVM, virtualization enabled in BIOS.

    ![](/img/Assignment7/4.PNG)

4. Do I have hardware AES/AES-NI advanced encryption support?
    - aes – Applications performing encryption and decryption using the Advanced Encryption Standard on Intel and AMD cpus.

    ![](/img/Assignment7/5.PNG)

## Install kvm-ok on a Debian/Ubuntu ##

1. sudo apt install cpu-checker

    ![](/img/Assignment7/6.PNG)

2. sudo kvm-ok

    ![](/img/Assignment7/7.PNG)

## Linux Virtualization Exercise ##

*** new Ubuntu 24.04 Linux installation, that supports nested virtualization.***
### Part 1: Introduction to virtualization concepts (30 minutes) ###

1. Introduction to Virtualization

- Virtualization is the process of creating multiple simulated computing environments on a single physical machine, allowing better resource utilization, isolation, and flexibility. Traditionally, one operating system (OS) runs on one physical machine. With virtualization, a single physical machine can run multiple virtual machines (VMs) or containers, each with its own OS or application environment.

2. Key Benefits of Virtualization:

- Better Resource Utilization – Multiple workloads share the same hardware.
- Cost Efficiency – Reduces the need for physical machines.
- Improved Scalability – Easy to create or remove VMs as needed.
- Isolation – Each VM or container is independent, preventing one failure from affecting others.


3. Types of Hypervisors

- Type 1 (Bare-Metal Hypervisor) – Runs directly on hardware (e.g., VMware ESXi, Microsoft Hyper-V, KVM).
- Type 2 (Hosted Hypervisor) – Runs inside an existing OS (e.g., VirtualBox, VMware Workstation, QEMU).

4. VM Tools in the Ubuntu Space

- KVM (Kernel-based Virtual Machine)
- Built into the Linux Kernel.
- Provides full virtualization, meaning it can run Windows, Linux, and other OSes as guests.
- Managed using virsh, virt-manager, or libvirt.
- Install KVM on Ubuntu:

4. Containers

- Containers provide a lightweight alternative to VMs by isolating applications using the same OS kernel.
- Containers share the host OS infrastructure but maintain their own unique setup.

5. Why Use Containers Instead of VMs?
    |Feature |	Virtual Machines |	Containers |
    |Isolation |	Strong (full OS per VM) |	Weaker (shared OS kernel) |
    |Performance |	Slower (OS overhead) |	Faster (lightweight) |
    |Resource Usage |	High (each VM needs its own OS) |	Low (containers share the OS) |
    |Startup Time |	Minutes |	Seconds |



## Part 2: Working with Multipass (1-2 hours) ##

- Multipass is a lightweight and powerful virtualization tool for Linux that allows you to create, manage, and automate Ubuntu virtual machines (VMs).

1. Installing Multipass on Linux.
    - Install Snapd: Multipass is available as a snap package, so you need to have snapd installed

    ![](/img/Assignment7/8.PNG)

### Basic Multipass Commands ###

1. Launch a Virtual Machine: Create and start a new Ubuntu virtual machine with a specified name.
    - multipass launch --name Assignment7-vm

    ![](/img/Assignment7/9.PNG)

2. List Virtual Machines: Display a list of all virtual machines created with Multipass.
    - multipass list

    ![](/img/Assignment7/10.PNG)

3. Access a Virtual Machine: Open a shell session inside a running virtual machine.
    - multipass shell Assignment7-vm

    ![](/img/Assignment7/11.PNG)

4. multipass exec: Run the command on the instance.
    - multipass exec Assignment7-vm -- uname -a
    - Checking system information
    -The multipass exec command is a powerful way to manage your instances and automate tasks efficiently.

    ![](/img/Assignment7/13.PNG)

5. multipass stop: Stop the running instance.
    - multipass stop Assignment7-vm
    - This command will stop the specified instance, effectively shutting it down.

    ![](/img/Assignment7/14.PNG)

6. multipass delete: Delete the instance.
    - multipass delete Assignment7-vm

    ![](/img/Assignment7/15.PNG)

### Cloud-init:Study ###

1. Create a cloud-init configuration file to customize the installation of a new instance.

- Cloud-init is a tool used to automate the initial configuration of cloud instances. It allows you to customize various aspects of your virtual machine.

    ![](/img/Assignment7/18.PNG)

- cloud-init Configuration File.

    ![](/img/Assignment7/17.PNG)

    ![](/img/Assignment7/19.PNG)

    ![](/img/Assignment7/20.PNG)

### File sharing: Explore how to share files and folders between your host computer and Multipass instances ###

1. Create a shared folder on your Linux VM.

    ![](/img/Assignment7/21.PNG)

2. Mount the folder inside the Multipass instance.

    ![](/img/Assignment7/22.PNG)

3. Check the shared folder inside the instance.

    ![](/img/Assignment7/23.PNG)

4. Create sample.txt file inside VM and check availability inside custom-instance VM.

    ![](/img/Assignment7/24.PNG)


## Part 3: Exploring LXD  ##

### Installing and managing LXD / LXC system ###

1. update your system by using apt
    - sudo apt update && apt upgrade -y
    - sudo apt upgrade -y

     ![](/img/Assignment7/25.PNG)
    
     ![](/img/Assignment7/26.PNG)

     ![](/img/Assignment7/27.PNG)

2. check if you just updated your kernel or other systems needing a complete system reboot, and if so reboot:      
    - sudo reboot

    ![](/img/Assignment7/28.PNG)

3. Install Snap
    - sudo apt install snap -y

    ![](/img/Assignment7/29.PNG)

4. install lxd using snap
    - sudo snap install lxd

5. check lxd version and installation
    - lxd --version

6. check that your user belongs to LXD group: id, and look for LXD. If you do not find lxd group, add user to it:   
    - sudo usermod -aG lxd $USER

    ![](/img/Assignment7/30.PNG)

7. check lxc system for listing of machines and containers
    - lxc list

    ![](/img/Assignment7/31.PNG)

8. initialize xld, to configure system to your environment
- lxd init

    ![](/img/Assignment7/32.PNG)

9. Once lxd is initialized successfully, we can verify the information using following set of commands:
    - lxc profile list

    ![](/img/Assignment7/33.PNG)

10. In order to list all available images, run
    - lxc image list images

    ![](/img/Assignment7/34.PNG)

11. Create your first container
    - lxc launch ubuntu:24.04 demo-container

    ![](/img/Assignment7/35.PNG)

12. Access the console of container.
    -  lxc exec demo-container -- bash

    ![](/img/Assignment7/36.PNG)

## Part 4: How to Stick Apps with Docker ##

1. Installing and managing Docker engine based system

- Follow good instructions from Docker, at https://docs.docker.com/

    - lxc exe demo-container -- bash

    ![](/img/Assignment7/37.PNG)

    - curl -fsSL https://get.docker.com -o get-docker.sh
    - sudo groupadd docker
    - sudo usermod -aG docker $USER
    - newgrp docker

    ![](/img/Assignment7/38.PNG)

    - check docker version

    ![](/img/Assignment7/39.PNG)

### Run Nginx on docker ###

1. Get the latest Nginx
    - docker pull nginx

    ![](/img/Assignment7/41.PNG)

2. Start docker nginx image
    - docker run -p 80:80 nginx
    - docker run -d -p 80:80 nginx

    ![](/img/Assignment7/42.PNG)

3. Open the IP of our virtual machine we can see Nginx is running

    ![](/img/Assignment7/40.PNG)


### Part 5: Snaps for Self-Contained Applications (30 minutes) ###

- Snaps provides a way to package applications with their dependencies for consistent execution on Linux distributions.

- Research: Using sources, explore Snapcraft and the concept of Snaps.

- Experiment: Choose a simple app and compress it into Snap using Snapcraft. For more information, you can find "Snapcraft" - from the source.

1. Install snapcraft
    - sudo apt update 
    - sudo snap install snapcraft

    ![](/img/Assignment7/43.PNG)

2. Create the python file

    ![](/img/Assignment7/44.PNG)
    
3. Create the Snapcraft Configuration

    ![](/img/Assignment7/44.PNG)

5. Create the snapcraft
    - snapcraft

![](/img/Assignment7/45.PNG)

![](/img/Assignment7/46.PNG)

![](/img/Assignment7/47.PNG)


