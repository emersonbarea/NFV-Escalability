# 1 - VM

We choose to use [Fedora20 - netinst](http://fedoraproject.org/en/download-splash?file=http://download.fedoraproject.org/pub/fedora/linux/releases/20/Fedora/x86_64/iso/Fedora-20-x86_64-netinst.iso) to create the VM because  all modules and dependencies are totaly compatible with it. If you prefer, you can try to use other distro and contribute with us sending some success work.

## 1.1 – Create a VirtualBox VM

We used [Oracle VM VirtualBox](https://www.virtualbox.org/) to host the VM. We suggest these configuration bellow:

```
- Disk space: 20GB
- Memory: 8GB
- Processor: 4 or more cores
- Network: 3 NIC (1 to internet access (NAT) and 2 host-only or internal network)
```

## 1.2 – Install Fedora20

Install only a basic Fedora20 system and follow the steps bellow:

### 1.2.1 – OS Update and Basic Configuration

```
yum update
yum install net-tools
service NetworkManager stop
chkconfig NetworkManager off
chkconfig network on

yum groupinstall "Development Libraries" "Development Tools"

yum install transfig wget texi2html libaio-devel dev86 glibc-devel e2fsprogs-devel gitk iasl xz-devel \
bzip2-devel pciutils-libs pciutils-devel SDL-devel libX11-devel gtk2-devel bridge-utils pyxmlsec.x86_64 \
qemu-common qemu-img mercurial glibc-devel.i686 rpm-build libidn-devel texinfo graphviz gnutls-devel \
seabios-bin ipxe-roms-qemu yajl-devel checkpolicy ocaml ocaml-findlib mingw64-binutils libuuid-devel \
python-lxml.x86_64 ethtool
```

## 1.3 – Xen Intallation

Obs.: we used these references in this procedure:
[Compiling Xen](http://wiki.xen.org/wiki/Compiling_Xen)
[Information for build xen-4.3.2-6.fc20](http://koji.fedoraproject.org/koji/buildinfo?buildID=539318)
[Boot the Xen hypervisor by default in Fedora 17 with GRUB2](http://major.io/2012/07/16/boot-the-xen-hypervisor-by-default-in-fedora-17-with-grub-2/)


Install Xen:

```
wget https://kojipkgs.fedoraproject.org//packages/xen/4.3.3/5.fc20/src/xen-4.3.3-5.fc20.src.rpm
rpm -i xen-4.3.3-5.fc20.src.rpm
cd rpmbuild/SPECS/
rpmbuild -bb xen.spec
cd ../RPMS/x86_64/
rpm -Uvh *4.3.3-5*.rpm
```

Change boot menu and test Xe installation:

```
grep ^menuentry /boot/grub2/grub.cfg | cut -d "'" -f2
grub2-set-default 'Fedora, with Xen hypervisor'
grub2-editenv list
grub2-mkconfig -o /boot/grub2/grub.cfg
```





When you finished to create your Fedora20 VM, click [here](/2_install_ClickOS.md) to install [ClickOS](http://cnp.neclab.eu/getting-started#clickos) and others softwares needed.
