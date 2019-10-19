# Objective
As part of the effort to create a new Kubernetes cluster using vagrant and VirtualBox I wanted to create a Vagrant box with packer to have a replicable server as initial building block.

~Shamelessly stealing~ reusing code from:
Packer: https://github.com/geerlingguy/packer-ubuntu-1804
Vagrant and kubernetes: https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/
Any mistake is me not understanding the sources.

## Further work
You can use this project as it is, but to extract more value you have several options.
* If you want to make sure you understand Packer: Change the file names, output dirs, usename, and try to adapt the build file and afterwards, while you are fixing the resulting mess, you will find out some intrinsic conventions and interactions.

* You can forget about kubernetes and adapt the ansible provisioner to install anything you want on the image/vagrant box that suits better your use case

* The same approach can be used to create a AWS AMI or gcloud image (1*) adding the specific builders and post-processors (provisioners should be the same).
HINT: If you want to use more than one post-processor read the post-processing sequence definition in https://www.packer.io/docs/templates/post-processors.html

# Packer
We want to create a virtualbox machine, install the required packages  and save as a vagrant box  

## 1 Create a virtualbox image,
### Builder section
We want to create the image from the ISO, this is more cumbersome than starting from an existing box but makes sure we know and can reproduce all the changes applied if we ever have to rebuild the vagrant box.

I will explain the attribute-value pairs that I found more complex, the rest should be explained in https://www.packer.io/docs/builders/virtualbox-iso.html

`boot_command`: This is a funny one, basically we are giving instructions to leave the install UI and start in command line, difficult to explain but if you start the building process you can see in the VM screen all the steps included in this boot command one by one one ( do you remember LOGO and the forward 10 , turn left, etc etc? same idea xD )

About the line `" preseed/url=http://{{ .HTTPIP }}:{{ .HTTPPort }}/preseed.cfg<wait>"`
As part of the effort of having a non-interactive install we use a pressed.cfg file so all the install options are read from it. The builder will start a http server in VirtualBox and serve the file `preseed.cfg`, the location of this file is indicated by the attribute-value pair `"http_directory": "http",`
Preseeding is a long topic so I will avoid it and give you a link. https://help.ubuntu.com/lts/installation-guide/amd64/apb.html

`ssh_username` and `ssh_password` are necessary to create a user with sudo access so we can use the provisioners, if this box has ever to hit production you want to change the one we use here , that's for sure, also generate and add a ssh key

## 2 Install the packages required for k8s
### Provisioners section
#### Shell
`setup.sh`: We ensure that our user `ssh_username` does not need tty or to enter the password when using sudo in order to make ansible work

I wasn't sure if I needed `cleanup.sh` but so far the box without using it is 1.2 GB and with it 935MB so it stays.

#### Ansible:
Provision docker and kubernetes packages
We use `ansible` provisioning so you need ansible on your host/laptop, if you don't want to do that you can use `ansible-local` provisioner but that means that you have to install ansible on the virtualbox (probably using the `shell` provisioner.
Docker packages:
  - docker-ce
  - docker-ce-cli
  - containerd.io

Kubernetes packages
  - kubectl
  - kubeadm
  - kubelet

Mind you, we are only installing the packages, configuration will come later when we have instantiated and started the master/s and worker node/s as we need the IP
## 3 Save as a Vagrant box
### Post-processors section
Not a lot to say here, check the overrides if you are creating more than one artifact

You can also upload to vagrant cloud if you have a Vagrant Cloud account
In
```
"variables": {
  "cloud_token": "{{ env `VAGRANT_CLOUD_TOKEN` }}"
},
```
We read the vagrant cloud token from environment variable named VAGRANT_CLOUD_TOKEN with your token there, otherwise remove this part (use `export VAGRANT_CLOUD_TOKEN="xxxx"` or `VAGRANT_CLOUD_TOKEN="xxxx" build build-ubuntu-18.04-k8s.json`)
WARNING: you need to create a box in Vagrant Cloud Web UI before being able to upload it there, otherwise you will get this error `* Post-processor failed: Error retrieving box`.

Once uploaded, before being able to use  you will have to release it (go to vagrant cloud WebUI, find the box, click the release button)
