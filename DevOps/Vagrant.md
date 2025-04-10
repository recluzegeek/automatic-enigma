# Introduction

Infrastructure as code (IaC) is the process of managing and provisioning infrastructure (networks, virtual machines, load balancers, and connection topology) through CODE/Config Files, e.g. Vagrant for local, Terraform for Cloud, Ansible for Servers, and Cloudformation for AWS. Vagrant is a tool to automate the deployments, and provisioning of virtual machines. Some useful commands:

```bash

vagrant init box-name # initializes Vagrantfile
# after initialing vagrant, we need to be in the vm folder for executing the box related commands

vagrant halt # for stopping the vm

vagrant destroy # for deleting vm

vagrant status # for vm status

vagrant global-status # provide overview of all vms

```

## Vagrant Networking

Change the Vagrantfile for private ip, bridging network adapter, changing default provider from virtualbox to vmware or some others. Increase the RAM, or CPU cores.

## Vagrant Sync Directory

Default sync directory is mounted inside the vm at `/vagrant` and is pointing to the folder where the Vagrantfile resides on host machine. We can also update this setting in Vagrantfile via `config.vm.synced_folder`.

## Provisioning

Provisioning means executing commands or scripts the vm boots up first time (bootstrapping a vm). We can also do forced provisioning on an already created vm by `vagrant reload --provision` or `vagrant provision`. If the vm is not created, and we've
setup provisioning in the Vagrantfile, then provisioning will be done automatically when we run `vagrant up`.

## Multiple VMs inside single VagrantFile

`config.vm.define` for multi-machine setup, and along with the help of ChatGPT
