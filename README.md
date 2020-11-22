# Vagrant PXE test environment

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Vagrant.png/394px-Vagrant.png" width="100" />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://ipxe.org/_media/logos/ipxe-small.png" width="90" />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/240px-Logo-ubuntu_cof-orange-hex.svg.png" width="100" />
</p>

A vagrant PXE client/server environment which supports legacy and UEFI boot with iPXE.  
It is designed to learn and test cloning solutions, nfsroot, ipxe, auto install etc...  
Default pxe configuration loads ipxe in UEFI mode and start ubuntu installer over nfsroot.  

### Prepare

Install qemu, libvirt, OVMF, nfsd, vagrant-libvirt
```
# For archlinux
pacman -S vagrant libvirt qemu ovmf virt-manager
sudo gpasswd -a $USER libvirt
systemctl start libvirtd
vagrant plugin install vagrant-libvirt
# choose libvirt in menu
```

### Server

- OS: debian 10  
- CPU: 1  
- RAM: 1024  
- eth0: Management network  
- eth1: Private network "pxe"  

### Client

- OS: debian 10  
- CPU: 1  
- RAM: 2048  
- eth0: Network to provide internet access  
- eth1: Private network "pxe"  

### Run

Run server with
```
$ vagrant up
```

To run client, which has ``autostart off``  
Note: If you want to test a legacy client boot, comment ``libvirt.firmware`` line in [Vagrantfile](Vagrantfile).
```
$ vagrant up client
```
Then open ``virt-manager`` to see your pxe client booting.

### Clean
```
vagrant destroy -f
```

**Refs**

- http://www.syslinux.org/wiki/index.php?title=PXELINUX
- https://help.ubuntu.com/community/DisklessUbuntuHowto
- https://github.com/vagrant-libvirt/vagrant-libvirt#no-box-and-pxe-boot
- https://github.com/stephenrlouie/PXE-Boot-VM/  
- https://ipxe.org
