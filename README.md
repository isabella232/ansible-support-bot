# Set up Support Bot servers with Ansible

This ansible playbook is layed out to install 2 servers (1x webserver, 1x database server).

Copy `vars.yml.example` to `vars.yml` and adjust the settings.

At the moment it's only possible to set them up using vultr (for production) and vagrant (for local testing).

NOTE: When installing the web server, the ansible playbook might fail when trying to restart nginx.
Simply SSH into your machine and kill any running nginx process, then rerun the playbook.

## Using vultr

Requirements:
- An API key from vultr.
- Ability to set up 2 instances on vultr.

### Install necessary Python modules for vultr module
$ pip install requests

### Run provisioning
$ ansible-playbook -u root provision.yml

### Set firewall groups on vultr.com
As this isn't possible yet with the `vultr` module, it must be done manually.
Since we're also setting up the firewall in the OS itself, this isn't absolutely necessary, but I don't think it will harm to have an extra layer.

## Using vagrant
For vagrant, a simple `vagrant up` should do the trick.
If you want to modify the IPs of the machines, do that in `Vagrantfile`.
