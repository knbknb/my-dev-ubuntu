# TO DO :point_right:  The Vagrant file has not fully been updated yet!

... and the helper scripts have not beeen touched either. Coming soon...

## my-ubuntu1804-4dev

This is a `Vagrantfile` for the base box configured in my [knbknb/ubuntu1804-4dev](https://github.com/knbknb/ubuntu1804-4dev) Github-repo.

### Requisites

- VirtualBox ([download, 90 MB](https://www.virtualbox.org/wiki/Downloads)) - a free and open-source hosted hypervisor for x86 virtualization
- Vagrant ([download, 40MB](https://www.vagrantup.com/downloads.html)) - for building and maintaining portable virtual software development environments

- Disk space: Enough free space on your hard disk or network drive whre you want to store the image. The Vagrant software will download a 3.5 GB Ubuntu image from the Vagrant Cloud, and cache it in directory `$HOME/.vagrant.d/boxes/`, and it will create a working copy from that image (so you need at least 2 * 3.5 = 7 GB)
- Network resources: Enough network bandwidth and enough download capacity remaining (if you are on a mobile plan)

#### Usage

Clone the repository, start a new terminal there, and run:

```sh
vagrant up
```
