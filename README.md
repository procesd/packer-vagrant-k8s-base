# Exercise
Create a new kubernetes cluster using vagrant and virtualbox. While there are examples in the internet that give you a easier route the main purpose here is to learn all the different steps and tools

* Steps
 * Packer: Obtain an ubuntu 18.04 k8 ready vagrantfile
   * Install ubuntu 18.04 in virtualbox
   * Provision docker and kubernetes packages
   * Export as a base box,

 * Vagrant: We have a single VM vagrantfile, we want at least 2 (1+ master and 1+ node) for the k8s cluster
   * Manually :( create a multimachine vagrant file that starts the 4 servers?   
 * Ansible: Create and configure the K8 cluster and dashboard
   * TODO: from single machines vagrantfiles how to start a cluster? or how to write a multimachine vagrantfile? I would say it's better to create the vagrantfile manually and use ansible to provision

# Packer
With packer we want to create a virtualbox machine, install the required packages (docker , kubectl, kubeadm, kubelet, python-pip?) and save it
## 1 Create a virtualbox image,
### Builder section

## 2 install the k8 master packages ?
### Provisioners section

## 3 save as a vagrantfile
### Post-processors section
