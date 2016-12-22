```bash
                                                         __                        
  ____  _________ ___        ___  _  ______  ____  _____/ /_      ____  ____  _____
 / __ \/ ___/ __ `__ \______/ _ \| |/_/ __ \/ __ \/ ___/ __/_____/ __ \/ __ \/ ___/
/ /_/ (__  ) / / / / /_____/  __/>  </ /_/ / /_/ / /  / /_/_____/ /_/ / /_/ (__  ) 
\____/____/_/ /_/ /_/      \___/_/|_/ .___/\____/_/   \__/      \____/ .___/____/  
                                   /_/                              /_/            
                                   
```

## Overview
This tool is to help deploy and provision [osm-export-tool2](https://github.com/hotosm/osm-export-tool2) development or production sites. Currently the base image is still Ubuntu 14.04 Trusty Tahr.

## Getting Started with Development Box using Virtualbox and Vagrant

Currently the provisioning workflow closely follows the operations described in the
[osm-export-tool2 README](https://github.com/hotosm/osm-export-tool2/blob/master/README.md), which it will diverge from in the future

0. Install [Vagrant](https://www.vagrantup.com/) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads) for your system. The provisioning was tested
on the following versions. So it's probably best to use these versions or greater:

    ```bash
    - Vagrant 1.9.1
    - VirtualBox 4.3.36
    - VBoxGuestAdditions 4.3.36
    - Ubuntu 14.04 LTS ( trusty ) # just so we're clear what my host OS was
    ```
0. After Vagrant is installed on your host make sure to install a few plugins:

    ```bash
    $ vagrant plugin install vagrant-vbguest
    $ vagrant plugin install vagrant-ansible-local
    $ vagrant plugin install vagrant-hostmanager
    ```

0. Review the default config variables in `/ops/group_vars/all.yml` that setup this environment. You might want to change some

0. Provision it by running the following commands. Vagrant and VboxGuestAdditions haven't been playing nicely on multiple platforms. If you see an error such as `Failed to mount folders in Linux guest` then check out the [workarounds](https://github.com/mitchellh/vagrant/issues/3341). A `DB_PASSWORD` environment variable is required to be set when kicking off `vagrant up`. A `CONFIG_ENVIRONMENT=prod` can be set if you want to use the production `settings.py` file.

    ```bash
    $ cd osm-export-ops/
    # or run `CONFIG_ENVIRONMENT=dev DB_PASSWORD=password vagrant provision` if the box is already up
    $ CONFIG_ENVIRONMENT=dev DB_PASSWORD=password vagrant up 
    ```

0. The `osm-export-tool2` repository should be checked out inside the `/dev` directory so you can edit the files locally.
The site will be running locally at `http://localhost:8080`.

0. The `nginx` server on port 8080 hands back statics and proxies requests to `gunicorn` on port 8100.
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

0. For debugging, if you are going to use `pdb` or `ipdb` ( it's already installed as part of `requirements-dev.txt` ) then make sure you stop the `gunicorn` server with `sudo service osm-export-tool2 stop` and also use Django `runserver` with `--nothreading` option. If you start `runserver` on port `8001` then nginx will proxy to `runserver`

    ```bash
    $ python -m pdb manage.py runserver --nothreading --noreload 8001
    ```

0. Some troubleshooting advice if things don't seem to be working properly:

   ```bash
   1. check that the services are running ( see above )
   2. check the various logs. hopefully these can be combined in the future into one stream:
      tail -f /var/log/osm-export-tool2.log
      tail -f /<path-to-osm-export-tool2-project>/debug.log
      tail -f /var/log/nginx/errror.log
   ```
   
## Getting Started with AWS

This part is almost done. 

Note: the AmericanRedCross team forked this repository and created an AWS version for deploying the site [over here](https://github.com/AmericanRedCross/osm-export-ops/commit/1ffd7ee6b86c1c59933c66db8a935d74cf9c5a0f)


## Getting Started with Development Box using Virtualbox and Vagrant

0. To be continued...maybe by you

