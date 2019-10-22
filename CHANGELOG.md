
0.0.3
=====
Fixed bug 0001,0002 was not a bug, we are jumping 2 versions because I don't seem to remember that the owner is at least as important as the permissions in ssh keys 


BUGS:
0001 - Add insecure public key in ~vagrant/.ssh/authorized_keys (proper permissions) as explained in https://www.vagrantup.com/docs/boxes/base.html

```
By default, Vagrant expects a "vagrant" user to SSH into the machine as. This user should be setup with the insecure keypair that Vagrant uses as a default to attempt to SSH. Also, even though Vagrant uses key-based authentication by default, it is a general convention to set the password for the "vagrant" user to "vagrant". This lets people login as that user manually if they need to.
```
Insecure key-par from  https://github.com/hashicorp/vagrant/tree/master/keys

Adding them to ansible provisioning script

0002
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

0.0.1
=====
Initial release
