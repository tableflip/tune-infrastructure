# TUNE Infrastructure

**Ansible scripts for deploying and maintaining tune.tableflip.io**

```sh
├── group_vars         # Config options
├── dev                # Inventory for local vm
├── next               # Inventory for staging vm
├── production         # Inventory for LIVE vm
├── roles              # Tasks, grouped by purpose
├── bootstrap.yml      # Playbook to get a fresh vm ready for Ansible
├── deploy-app.yml     # Playbook to deploy & update the app
└── Vagrantfile        # Test the scripts locally with `vagrant up`
```

## Prerequisites

You need to add a `secrets.yml` file into `group_vars/all`.

This file contains all of the secrets for the deployments that we'd prefer not to keep in the repo.
## Usage

**To bootstrap a local test server with vagrant**

- Install Ansible
- Install Vagrant (`brew install vagrant`)
- Add `10.100.112.100	dev.tune.tableflip.io` to your local `/etc/hosts`

```sh
# Download and provision a vm
vagrant up

# Update vm with our roles
ansible-playbook -i dev playbook.yml
```

You now have a test vm, running locally

**To bootstrap a new production vm**

- Add the new remote to the relevant inventory
- Add your public ssh key in `/root/.ssh/authorized_keys` on the remote

```sh
# bootstrap ansible user
ansible-playbook -i production bootstrap.yml --extra-vars "ansible_ssh_user=root"

# Intall app and dependencies
ansible-playbook -i production playbook.yml
```
