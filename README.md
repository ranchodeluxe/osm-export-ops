## What?
A repository that helps build a development environment
for the [osm-export-tool2](https://github.com/hotosm/osm-export-tool2) using Ansible, Vagrant, Virtualbox and Ubuntu 14.04 . Later it will be generalized
to help with EC2 deployment. Currently the provisiong workflow follows the order of operations described in the
[osm-export-tool2 setup README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md) which it probably shouldn't in the future

## Getting Started

0. Install [Vagrant](https://www.vagrantup.com/) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads) for your system. The provisioning was tested
on the following versions. So it's probably best to use these versions or greater:

    ```bash
    - Vagrant 1.8.1
    - VirtualBox 4.3.10
    - VBoxGuestAdditions 4.3.10
    - Ubuntu 14.04 LTS ( trusty ) # just so we're clear what my host OS was
    ```
0. After Vagrant is installed on your host make sure to install a couple plugins:

    ```bash
    $ vagrant plugin install vagrant-vbguest
    $ vagrant plugin install vagrant-ansible-local
    ```

## TODO

0. make it virtualenv compatible if flag set

0. make Celery run as daemon so we don't have to ssh in and run screen

0. generalize this to work with EC2 deployment and lock down user roles

0. reorganization the provisioning workflow to make more sense instead of following setup directions in
[README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md)