# Quick-VM
Setup a Windows VM very easily and quickly on any Arch, Debian or Fedora system using Red Hat KVM. 

### Requirements:
 
  - Ubuntu 18.04 or newer
  - Fedora 29 or newer
  - Arch (You can read this [Guide by LinuxHint](https://linuxhint.com/install_configure_kvm_archlinux) for permissions and User Group setup)
  - 4 CPUs (2 Hyperthreaded Cores at minimum, 1 vCPU is reserved for the host)
  - 8 GiB of RAM 

<p>
<details>
<summary>Installing Dependencies</summary>
 
<br>
 
### Install Qemu, Virt-Manager, Libvirt and other dependencies depending on your distro.
 First up, you must install KVM and the Virtual Machine Manager. By installing `virt-manager`, you will get everything you need for your distribution:
 
 ```bash
 
 # Debian & Ubuntu based ditros 
 sudo apt install -y qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager
``` 

 ```bash
 # Fedora based ditros  
 sudo dnf -y install bridge-utils libvirt virt-install qemu-kvm
``` 

```bash
 # Arch based ditros 
 sudo pacman -S --noconfirm virt-manager qemu vde2 ebtables dnsmasq ridge-utils openbsd-netcat
```

### After installing the dependencies, make sure you enable libvirtd.service
```bash
 # Enable Libvirt Service
 sudo systemctl enable --now libvirtd
 ```
 
<br> 
</details>
</p>
 
 
**Note:** Any Linux distribution will work just fine. You do need to install `virt-manager`, `qemu`, and other required dependencies. ***Linux Kernel Version 5.4 LTS or newer is recommended.*** 

 
 ## Download the Windows 10 ISO and KVM VirtIO drivers
 You will need **Windows 10 Pro/Pro N**, as it has RDP Support which is needed if you want to run Windows Apps under Linux. You will also need drivers for VirtIO to ensure the best performance and lowest overhead for your system.
 
- [VirtIO Drivers](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso)
- [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)

**Note:** You have to place the ISOs in your `~/WindowsVM` directory, as it will be clutter-free and easy to locate. Also, the KVM config file points to that directory to find those ISOs.

### Make sure you rename both of the ISOs as following:

> **Windows 10 ISO**: `win10vanilla.iso`
> **VirtIO Drivers**: `virtio-win.iso`

### Specs of the VM are as follows:

>**CPU**: 3 vCPUs Allocated.

>**Memory**: Total 6144 MiB, 1024 Allocated initially.

>**Primary Drive**: 256 GB VirtIO Disk (Dynamically Allocated)

>**Secondary Drive**: Windows 10 ISO (SATA CDROM)

>**Other Drives**: VirtIO Drivers ISO (SATA CDROM), Essential Tools ISO (to optimize VM performance)

>**Network Card**: VirtIO (Disabled by default, Recommended this way until debloated)
