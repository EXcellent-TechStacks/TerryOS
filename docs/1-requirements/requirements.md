# Running XorinOS on a virtual machine

The recommendation here suggests that you run Xorin within a controlled virtual machine environment. Here are the steps and requirements in order to do so: 

## Requirements
- A build-ready `.iso` image of Xorin-OS. 
- An installed version of QEMU (Quick-EMUlator). 

## Installation and Run

```
qemu-system-x86_64 \
    -m 4096 \
    -smp 4 \
    -cdrom Xorin-OS.iso \
    -boot order=d \
    -machine q35 \
    -accel kvm
```

This should start a virtual machine with the following resources: 
- 4 GB RAM
- 4 virtual CPU cores
- Q35 virtual motherboard
- KVM hardware acceleration

However, booting from a live `.iso` should be temporary if you want long-standing usage. Thus, the following command: 
```
qemu-img create -f qcow2 xorin-disk.qcow2 40G
```
creates a virtual 40 GB disk. The file initially occupies much less space and grows as data is written.

#### Start the installer:
```
qemu-system-x86_64 \
    -m 4096 \
    -smp 4 \
    -machine q35 \
    -accel kvm \
    -drive file=xorin-disk.qcow2,format=qcow2 \
    -cdrom Xorin-OS.iso \
    -boot order=d
```
The installer should detect the virtual disk and allow the user to install Xorin-OS onto it.

#### Run the installed OS
After installation, shut down the VM and start it without the ISO:

```
qemu-system-x86_64 \
    -m 4096 \
    -smp 4 \
    -machine q35 \
    -accel kvm \
    -drive file=xorin-disk.qcow2,format=qcow2
```

The VM now boots from its virtual hard drive.