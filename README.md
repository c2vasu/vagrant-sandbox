# vagrant-sandbox

A sample Vagrantfile to set up a CentOS box with a docker-engine, aws cli, Kubectl


## Requirements

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/)

## Usage

### To Start

```
$ vagrant up   # This takes a few minutes to complete.
$ vagrant ssh
```

### Directory is synced (i.e shared folder)

This directory is synced to `/vagrant` in the guest VM

```
$ cd /vagrant
```

### To Shut down
```
$ vagrant halt
```

## Trouble shooting

### Unable to mount
Vagrant was unable to mount VirtualBox shared folders.
# https://github.com/aidanns/vagrant-reload/issues/4
```
$ vagrant plugin install vagrant-vbguest
$ vagrant vbguest
```
