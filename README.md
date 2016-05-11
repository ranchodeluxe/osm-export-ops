## Getting Started with Virtualbox

A repository that helps build a development environment
for [osm-export-tool2](https://github.com/hotosm/osm-export-tool2) using Ansible, Vagrant, Virtualbox and Ubuntu 14.04 . Later it will be generalized
to help with EC2 deployment. Currently the provisioning workflow closely follows the operations described in the
[osm-export-tool2 README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md), which it will diverge from in the future

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

0. When provisioning finishes the `osm-export-tool2` repository should be inside `dev/` so you can edit the files locally

0. The `nginx` server on port 80 hands back statics and proxies requests to `gunicorn` on port 81. You can interact with the osm-export-tool2 `gunicorn` service like:

    ```bash
    $ sudo service osm-export-tool2 status # is it up or down?
    $ sudo service osm-export-tool2 start
    $ sudo service osm-export-tool2 stop
    ```

0. The `celery` and `celerybeat` daemons can also be inspected this way:

    ```bash
    $ sudo service celery status # is it up or down?
    $ sudo service celerybeat status # is it up or down?
    ```

0. If you are are developing using the Django runserver remember to stop the `gunicorn` service with `sudo service osm-export-tool2 stop`

## Getting Started with AWS

0. To be continued...