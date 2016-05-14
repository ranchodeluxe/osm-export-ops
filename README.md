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
0. After Vagrant is installed on your host make sure to install a few plugins:

    ```bash
    $ vagrant plugin install vagrant-vbguest
    $ vagrant plugin install vagrant-ansible-local
    $ vagrant plugin install vagrant-hostmanager
    ```
0. Export an environment variable called `DB_PASSWORD`. If this is not set then the `vagrant up` command will raise an exception later on

    ```bash
    $ export DB_PASSWORD=password
    ```

0. Review the default config variables in `/ops/group_vars/all.yml` that setup this environment. You might want to change some

0. Provision it by running the following commands. Vagrant and VboxGuestAdditions haven't been playing nicely on multiple platforms.
If you see an error such as `Failed to mount folders in Linux guest` then check out the [workarounds](https://github.com/mitchellh/vagrant/issues/3341)

    ```bash
    $ cd osm-export-ops/
    # NOTE: i'm creating the VM with administrative rights (sudo)
    # so that Vagrant can proxy host port 80 nicely, you should do the same
    $ sudo -E vagrant up
    ```

0. The `osm-export-tool2` repository should be inside the `/dev` directory so you can edit the files locally.
The site *should* be running locally at `http://localhost:80`.

0. The `nginx` server on port 80 hands back statics and proxies requests to `gunicorn` on port 81.
You can interact with the osm-export-tool2 `gunicorn` service (after sshing into the vagrant instance) with:

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

0. Some troubleshooting advice if things don't seem to be working properly:

   ```bash
   1. check that the services are running ( see above )
   2. check the various logs. hopefully these can be combined in the future into one stream:
      tail -f /var/log/osm-export-tool2.log
      tail -f /<path-to-osm-export-tool2-project>/debug.log
      tail -f /var/log/nginx/errror.log
   ```

## Getting Started with AWS

0. To be continued...
