0.0.5
=====
Adding daemon.json file (docker daemon configuration) to use the systemd cgroups driver as recommended in https://kubernetes.io/docs/setup/production-environment/container-runtimes/
(bug 0005)


0.0.4
=====
Fixing kubeadm version to 1.1.6 and then all the rest of k8s and docker versions to validated ones.

- kubelet=1.16.*
- kubeadm=1.16.*
- kubectl=1.16.*
and
- docker-ce=5:18.09.*
- docker-ce-cli=5:18.09.*
- containerd.io=1.2.6*


0.0.3
=====
Fixed bug 0001. We are jumping 2 versions because I don't seem to remember that the owner is at least as important as the permissions in ssh keys
0002 was not a bug, as it looks like the only way of not getting a missmatch is adding the virtualbox guest additions version that matches the virtualbox encironment were we run the box, which will change over time.

0.0.1
=====
Initial release
