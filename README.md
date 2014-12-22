# Bootstrap a Go CD Network

This project uses Vagrant to provision a Go (http://www.go.cd/) server and agents.

## Prerequisites

You must have Vagrant (https://www.vagrantup.com/) installed.

The Vagrant provisioner is Ansible (http://www.ansible.com) and can be installed on Mac OS X using homebrew with:

```
$ brew install ansible
```

You also need to have downloaded the .deb files for the Go server and agent (see http://www.go.cd/download/). The Vagrantfile assumes you will have a ~/Downloads/go directoy that contains those .deb files. Vagrant mounts that directory as a shared directory, /downloads. Ansible will use /downloads when installing the packages.

## Usage

Clone this repository, then run:

```
$ vagrant up
```

This by default will provision the server and 2 agent nodes. When this is finished, point your browser to https://localhost:8154. You can see your agents waiting to be enabled here: https://localhost:8154/go/agents.
