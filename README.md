[![GitHub](https://img.shields.io/badge/Quick-VM-brightgreen?style=for-the-badge&logo=Material-Design-Icons&logoColor=white)](https://github.com/gamerhat18/Quick-VM/)
![Arch](https://img.shields.io/badge/Arch-blue?style=for-the-badge&logo=Arch-Linux&logoColor=white)
![Fedora](https://img.shields.io/badge/Fedora-blue?style=for-the-badge&logo=Fedora&logoColor=white&color=294172)
![Debian](https://img.shields.io/badge/Debain-red?style=for-the-badge&logo=Debian&logoColor=white&color=A81D33)
![Ubuntu](https://img.shields.io/badge/Ubuntu-orange?style=for-the-badge&logo=Ubuntu&logoColor=white&color=E95420)
![Terminal](https://img.shields.io/badge/Terminal-black?style=for-the-badge&logo=Windows-Terminal&logoColor=white&color=4D4D4D)
![Windows](https://img.shields.io/badge/Windows-blue?style=for-the-badge&logo=Windows&logoColor=white&color=0078D6)


# Quick-VM (WORK IN PROGRESS) 

Setup a Windows VM very easily and quickly on any Arch, Debian or Fedora system using RedHat KVM. 

>Now that [NVIDIA has enabled GPU Passthrough for Windows based Virtual Machines](https://nvidia.custhelp.com/app/answers/detail/a_id/5173) (no more Code 43!), this project will also include easy setup and configuration for passing through NVIDIA GPUs in KVM and interfacing Windows apps from linux via [WinApps](https://github.com/Fmstrat/winapps/).
>
>Till then, read [Arch Wiki - PCI Passthrough](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF) and [this]().

# 🚀 Getting Started

### 🌟 Simple Install:

1. Download [Windows 10 Pro ISO](https://www.microsoft.com/en-us/software-download/windows10ISO), and [VirtIO Drivers (Stable)](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso)

2. Place the ISOs in either `~/WindowsVM/` or `/var/lib/libvirt/images/`.

3. Rename the ISOs as shown below:
    - **Windows 10 ISO** ➜ `win10.iso`
    - **VirtIO Drivers** ➜ `virtio-win.iso`

4. Open your terminal and enter the command shown below  

### 🔩 One-liner to Setup KVM - Paste this in your terminal

```bash
bash <(curl -sL https://git.io/JOeOs) 
 ```

> Here is the [Script](https://github.com/thegamerhat/quick-vm/blob/main/one-liner.sh)

---

### ✅ Getting the VM Ready:

<p>
<details>
<summary>Step by Step Guide - Click Me!</summary>
<br>

### Please follow along the screenshots below to get the VM ready.

+ Click inside the VM Window and press any key when asked.  

![Screenshot](docs/img/first-boot.png)

![Screenshot](docs/img/booting-iso.png)

+ Select your language and keyboard input and click Next.

![Screenshot](docs/img/install-1.png)

![Screenshot](docs/img/install-2.png)

+ Enter your product key now, or you can skip and enter your product key after installation.

![Screenshot](docs/img/install-3.png)

+ Selecting **Windows 10 Pro N** will install **Windows 10 Pro** without extra bloat.
> Note: Choose Windows 10 Pro XXX or Enterprise if you need Hyper-V for Stealth VM. 

![Screenshot](docs/img/install-4.png)

![Screenshot](docs/img/install-5.png)

+ Select **Custom Install**  (because the other one is useless)

![Screenshot](docs/img/install-6.png)

+ Click on **Load Driver** to install disk drivers.

![Screenshot](docs/img/install-7.png)

![Screenshot](docs/img/install-8.png)

+ Double-Click on **CD Drive virtio-win** ➜ **amd64** ➜ **w10** and click **OK**. 

![Screenshot](docs/img/install-9.png)

+ Just click N**ext** to select the default one.

![Screenshot](docs/img/install-10.png)

+ Select `Unallocated Space` and click **Next** to begin the installation.

![Screenshot](docs/img/install-11.png)

![Screenshot](docs/img/install-12.png)


</br>
</details>
</p>

### 🖥 Host System Requirements:
 
  - **Ubuntu 18.04** or newer
  - **Fedora 31** or newer
  - **Arch** (Read this [Guide by LinuxHint](https://linuxhint.com/install_configure_kvm_archlinux) for permissions and User Group setting)
  - **4 CPUs** (2 Multi-Threaded Cores at minimum)
  - **8 GiB Memory** (more = better)
  - **40+ GiB of Free Storage** typically (**SSD Recommened**)
  
> **Linux Kernel 5.4 LTS** or newer is recommended 

<p>
<details>
<summary>Default specs of the VM</summary>
<br>

**CPU**: 4 vCPUs Allocated

**GPU**: VirtIO or [VFIO GPU Passthrough - ArchWiki](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF) or [Single-GPU-Passthrough](https://github.com/joeknock90/Single-GPU-Passthrough) 

**Memory**: Total 6 GiB, 1 GiB Allocated initially

**Storage Drive**: 1 TB VirtIO Disk (Dynamically Allocated)

**DVD Drive**: Windows 10 ISO

**Other Drives**: VirtIO Drivers ISO, Essential Tools ISO (to optimize VM performance)

**Network Card**: VirtIO (Recommended Disabled until debloated)
</br>
</details>
</p>

## ⚔️ Advanced Install

### 🔖 [Making a Stealth VM](docs/stealth-vm.md)

### 🔖 [DIY VM (Everything From Scratch)](docs/diy-vm.md)

<p>
<details>
<summary>📍 Check KVM Compatibility</summary>
<br>

1. Checks if `AMD-V` or `VT-d`/`VT-x` is supported on your AMD/Intel CPU.
2. Checks if kvm is enabled using `virt-host-validate`.</br>
</details>
</p>

<p>
<details>
<summary>📍 Install required packages</summary>
<br>

- Updates repositories (Debian and Fedora only) and installs required packages.

```
# Debian
sudo apt update -q && sudo apt install -y qemu qemu-kvm libvirt-bin libvirt-daemon libvirt-clients bridge-utils virt-manager

# Fedora
sudo dnf -y install qemu-kvm libvirt bridge-utils virt-install virt-manager

# Arch
sudo pacman -S --noconfirm qemu libvirt bridge-utils edk2-ovmf vde2 ebtables dnsmasq openbsd-netcat virt-manager
```
</br>
</details>
</p>

<p>
<details>
<summary>📍 Enable Libvirt Service & Virtual Networking</summary>
<br>

**Executes the following commands only if systemd is present and running.**

```
# Libvirt service and socket
sudo systemctl enable --now libvirtd

# Virtlogd
sudo systemctl enable --now virtlogd

# Virtual Networking
sudo virsh net-autostart default
sudo virsh net-start default
```
</br>
</details>
</p>

<p>
<details>
<summary>📍 Locate ISOs</summary>
<br>

1. Checks if `win10.iso` and `virtio-win.iso` exist in ~/WindowsVM or /var/lib/libvirt/images
2. Uses `rsync` to copy the ISOs to /var/lib/libvirt/images (_$HOME subdirectories might cause permission issues_)

</br>
</details>
</p>

<p>
<details>
<summary>📍 Selecting a VM Profile</summary>
<br>
1. Serious Business - Ideal

```
Specs: 
6 CPU Threads
8 GB RAM
```

2. Decently Powerful [Default]

```
Specs: 
4 CPU Threads
6 GB RAM
```

3. Lightweight and Barebones

```
Specs: 
2 CPU Threads
4 GB RAM
```

4. Stealth VM (Beta) - Ideal for DRM/Anticheat Programs

```
Specs: 
4 CPU Threads
6 GB RAM
```
</br>
</details>
</p>

<p>
<details>
<summary>📍 Reload KVM Kernel Modules</summary>
<br>

If `kvm` is enabled correctly, then executes the following commands depending upon the CPU.

```
# AMD
sudo modprobe -r kvm_amd kvm      # safely unloads the modules
sudo modprobe kvm                 # enables kvm first
sudo modprobe kvm_amd nested=1    # then kvm_amd module with nested enabled

# INTEL
sudo modprobe -r kvm_intel kvm    # safely unloads the modules
sudo modprobe kvm                 # enables kvm first
sudo modprobe kvm_intel nested=   # then kvm_intel module with nested enabled
```
</br>
</details>
</p>

## Some more useful stuff

1. Single GPU Passthrough - [SomeOrdinaryGamers](https://youtube.com/watch?v=BUSrdUoedTo), [joeknock90](https://github.com/joeknock90/Single-GPU-Passthrough) and [Stetsed](https://github.com/Stetsed/NVIDIA-Single-GPU-Passthrough)
2. 

## 🎁 Contribute/Help

**Stuff changes all the time, and I am only a kid maintaining this repo.**

**If you want to help, THANK YOU, that will be wonderful!** 💜

**This project is aimed at making a Windows VM for gaming/content creation, paired with [WinApps](https://github.com/Fmstrat/winapps/) to make it easier for people to switch to Linux while still being able to use their Adobe CC apps, MS Office, Play Games etc.**

## 📣 Credits

- SomeOrdinaryGamers - For [Video guide](https://youtube.com/watch?v=BUSrdUoedTo)
- Zeptic - For [Stealth VM stuff](https://youtube.com/watch?v=VKh2eKPnmXs)

## 📬 Contact me

![visitors](https://visitor-badge.glitch.me/badge?page_id=gamerhat18.quick-vm)
[![Mail Me](https://img.shields.io/badge/Gmail-%23EA4335.svg?&style=flat-square&logo=gmail&logoColor=white)](mailto:gamerhat18@gmail.com) 
[![Secure Mail Me](https://img.shields.io/badge/Proton-Mail-%23663399.svg?&style=flat-square&logo=Proton-Mail&logoColor=white)](mailto:gamerhat18@protonmail.com) 
[![GitHub](https://img.shields.io/badge/GitHub-%23181717.svg?&style=flat-square&logo=github&logoColor=white)](https://github.com/thegamerhat) 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?&style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/pranav-kulkarni-94b975180) 
[![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?&style=flat-square&logo=twitter&logoColor=white)](https://twitter.com/gamerhat18)

