A repository that helps build a development environment
for [osm-export-tool2](https://github.com/hotosm/osm-export-tool2) using Ansible, Vagrant, Virtualbox and Ubuntu 14.04 . Later it will be generalized
to help with EC2 deployment. Currently the provisioning workflow closely follows the operations described in the
[osm-export-tool2 README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md), which it will diverge from in the future

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
0. Review the default config variables in `/ops/group_vars/all.yml` that setup this environment. You might want to change some of these

0. Provision it by running the following commands. Vagrant and VboxGuestAdditions haven't been playing nicely on multiple platforms.
If you see an error such as `Failed to mount folders in Linux guest` then check out the [workarounds](https://github.com/mitchellh/vagrant/issues/3341)

    ```bash
    $ cd osm-export-ops/
    $ vagrant up
    ```
0. When provisioning finishes the `osm-export-tool2` repository should be inside `dev/` so you can edit the files locally:

0. Run the Django development server as described in `osm-export-tool2` docs. In the future this will be setup with Apache:

    ```bash
    $ cd osm-export-ops/ # on host system
    $ vagrant ssh
    $ sudo su - osmexport # or the name of your {{ app_user.user_name }} in config vars
    $ python manage.py runserver 0.0.0.0:8000
    ```

0. Run the celery workers as described in `osm-export-tool2` docs. In the future this will be daemonized:

    ```bash
    $ cd osm-export-ops/ # on host system
    $ vagrant ssh
    $ sudo screen
    $ sudo su - osmexport # or the name of your {{ app_user.user_name }} in config vars
    $ celery -A core worker --loglevel debug --logfile=celery.log
    ```


## TODO

0. make it virtualenv compatible if flag set

0. make the main site run on Apache instead of Django development server

0. make Celery run as daemon so we don't have to ssh in and run screen

0. generalize this to work with EC2 deployment and lock down user roles

0. reorganization the provisioning workflow to make more sense instead of following setup directions in
[README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md)