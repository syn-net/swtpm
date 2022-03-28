# Building swtpm

NOTE(jeff): This file derives from the original `INSTALL` file found in the root of this git repository. Without further ado, let us begin!

## usage

Building and running the swtpm has dependencies on the following packages:

- automake
- autoconf
- bash
- coreutils
- expect
- libtool
- sed
- libtpms
- libtpms-devel
- fuse
- fuse-devel
- glib2
- glib2-devel
- json-glib-devel
- net-tools
- python3
- python3-twisted
- selinux-policy-devel
- socat
- trousers
- gnutls
- gnutls-devel
- libtasn1
- libtasn1-tools
- libtasn1-devel
- rpm-build (to build RPMs)

Debian/Ubuntu also needs the following packages to build:

- build-essential
- devscripts
- equivs

On RHEL or Fedora use either one of the following methods to install
the above dependencies:

 - sudo dnf builddep ./swtpm.spec  (Fedora >= 22)
 - sudo yum install yum-utils ; sudo yum-builddep ./swtpm.spec  (RHEL and Fedora <= 21)
 - sudo yum install <package name(s)>

On Ubuntu use the following command:

 - sudo mk-build-deps --install ./debian/control


Use the following sequence to build and install the Software TPM.

./autogen.sh --prefix=/usr
make
make check
make install


To build an rpm on a Fedora or RHEL host do:

./autogen.sh
make dist
rpmbuild -ta swtpm-*.tar.gz


To build a Debian package on a Debian compatible host do:

echo "libtpms0 libtpms" > ./debian/shlibs.local
debuild -us -uc

## Ubuntu 20.04.3 LTS

For those of us with troubles installing the `swtpm-tools` from the official repositories, please try the [following directions](https://www.reddit.com/r/VFIO/comments/q49xb4/how_install_swtpm_tpm_20_for_ubuntu_impris_indri/hfxr484/?utm_source=share&utm_medium=web2x&context=3).

```shell
sudo apt install devscripts git equivs

git clone https://github.com/stefanberger/swtpm.git swtpm.git
cd swtpm.git
echo "libtpms0 libtpms" > ./debian/shlibs.local
sed -i 's/SWTPM_TEST_SECCOMP_OPT.*$/SWTPM_TEST_SECCOMP_OPT=""/' ./debian/rules
sudo mk-build-deps --install ./debian/control
debuild -b
sudo apt autoremove swtpm-build-deps
sudo apt install ../swtpm*.deb
```

### reference documents

1. [Tpm2 Device Emulation With Qemu](https://tpm2-software.github.io/2020/10/19/TPM2-Device-Emulation-With-QEMU.html)
1. [swtpm PPA](https://launchpad.net/~smoser/+archive/ubuntu/swtpm)
