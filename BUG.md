# BUGS:

## 0005 Using wrong cgroup manager
Kubeadm init was logging:
[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/

Systemd uses it's own cgroup driver while kubelet and docker(?) uses cgroupfs, from https://kubernetes.io/docs/setup/production-environment/container-runtimes/

```
 We have seen cases in the field where nodes that are configured to use cgroupfs for the kubelet and Docker, and systemd for the rest of the processes running on the node becomes unstable under resource pressure.
 Changing the settings such that your container runtime and kubelet use systemd as the cgroup driver stabilized the system.
 ```
We added a daemon.json file configuring docker to use systemd

## 0004 Using non validated versions
*Fixed in version:* 0.0.4
[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 19.03.4. Latest validated version: 18.09



## 0003 Warning on mismatched GuestAdditions
*Fixed in version:* 0.0.3
Even using plugin `vagrant-vbguest` we are still getting
```
[default] GuestAdditions versions on your host (6.0.12) and guest (5.2.8) do not match.
```
It's not causing an error now as the plugin updates guest additions but it would if the plugin was not installed, adding the option
`"guest_additions_path": "VBoxGuestAdditions_{{.Version}}.iso",` to the packer build file to see if that helps


## 0002 Not being able to mount/install host Additions
*Fixed in version:* Not really a bug, depends on vagrant plugin `vagrant-vbguest`


Getting:
```
==> default: Mounting shared folders...
    default: /vagrant => /Users/perecortada/vagrant/k8s
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

The error output from the command was:

mount: /vagrant: wrong fs type, bad option, bad superblock on vagrant, missing codepage or helper program, or other error.
```

Not sure what's that, but it must be me.
Mmm, trying `vagrant plugin install vagrant-vbguest` -> Worked


## 0001 - Add insecure public key in ~vagrant/.ssh/authorized_keys (proper permissions)
*Fixed in version:* 0.0.3

As explained in https://www.vagrantup.com/docs/boxes/base.html

```
By default, Vagrant expects a "vagrant" user to SSH into the machine as. This user should be setup with the insecure keypair that Vagrant uses as a default to attempt to SSH. Also, even though Vagrant uses key-based authentication by default, it is a general convention to set the password for the "vagrant" user to "vagrant". This lets people login as that user manually if they need to.
```
Insecure key-par from  https://github.com/hashicorp/vagrant/tree/master/keys

0.0.3 -> Adding them to ansible provisioning script
