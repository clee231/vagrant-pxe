# Vagrant PXE test environment

A vagrant PXE client/server environment which supports:
- virtualbox and libvirt providers.  
- legacy and UEFI boot
- pxelinux and ipxe

It is designed to learn and test cloning solutions, nfsroot, syslinux, ipxe etc...

## Setup

1° install Qemu, Libvirt, Virtualbox, OVMF  
 * See your distribution documentation.
2° if using, install [vagrant-libvirt provider](https://github.com/vagrant-libvirt/vagrant-libvirt)
3° ``git clone http://github.com/eoli3n/vagrant-pxe``

## Edit PXE configuration

PXE server's installation script is ``config/setup.sh``.  
All required files are in ``config/ressources``.  
Default pxe configuration loads ipxe in EFI mode.

## Run PXE server

* OS: debian 10
* CPU: 1
* RAM: 1024
* Networking
 * eth0: Management network
 * eth1: Private network "pxe_network"

### Virtualbox provider

```
$ cd vagrant-pxe/server
$ vagrant up --provider virtualbox
$ vagrant ssh
```

### Libvirt provider

```
$ cd vagrant-pxe/server
$ vagrant up --provider libvirt
$ vagrant ssh
```

## Run PXE client

* OS: debian 10
* CPU: 1
* RAM: 2048
* Networking
 * eth0: Private network "pxe_network"

### Virtualbox provider

A box needs [to be set](https://github.com/mitchellh/vagrant/issues/4487) for virtualbox provider.  
If changing, please choose one which supports virtualbox and libvirt providers.  

Requires [virtualbox extension pack](https://www.virtualbox.org/wiki/Downloads).

```
$ cd vagrant-pxe/client
$ vagrant up --provider virtualbox
```
Vagrant will hang on ``Warning: Connection refused. Retrying...`` error.
That's because we edited network configuration to enable pxeboot. Please ignore it.

Virtualbox gui will pop up, showing vm netboot.

### Libvirt provider

No box required, but will use ``Debian 8`` as with virtualbox provider.

```
$ cd vagrant-pxe/client
$ vagrant up --provider libvirt
```
Vagrant will hang on ``Waiting for domain to get an IP address...``.  
That's because box ask for network configuration on management network which do not exist here. Please ignore it.  

To restart boot procedure.
```
$ vagrant reload
```

**Refs**

* http://www.syslinux.org/wiki/index.php?title=PXELINUX
* https://help.ubuntu.com/community/DisklessUbuntuHowto
* https://github.com/vagrant-libvirt/vagrant-libvirt#no-box-and-pxe-boot
* https://github.com/stephenrlouie/PXE-Boot-VM/  
