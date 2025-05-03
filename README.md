# terraform-multipass-kubernetes

## EH Notes
NOTE: argally made fixes and this works on macOS
- based on a [blog](https://medium.com/globant/kubernetes-magic-0dd051f046e8) by Globant
- the [repo](https://github.com/smiguez85/terraform-kubernetes-multipass) listed in blog doesn't tf up successfly though
- this module doesn't delete multipass resources on terraform destroy - run the separate reset.sh script
- this multipass setup uses haproxy in front of api server and calico for networking

## Summary
Build a Local Kubernetes cluster the easiest way.

The cluster is built using [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/), providing 1 control-plane node and 3 worker nodes, although you can customize this setup.
## Prerequisite:
* [Terraform](https://developer.hashicorp.com/terraform/downloads?product_intent=terraform)
* [Multipass](https://multipass.run/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

## Setup / Use
### SSH file
must have an rsa pub key
```sh
cd $HOME/.ssh
ssh-keygen -t rsa -b 2048
```

### Terraform setup
to test locally - create a `versions.tf` and use a *-local type project / workspace on tf cloud
all the resources will be local and throwaway anyways

I haven't tested this yet beyond the simple master / worker setup

### Cleanup
the terraform doesn't actually clean up the multipass resources
run `bash reset.sh` afterwards to clean up multipass and tf generated config resources

this is multipass - so it'll leave orphan'd dhcp lease records. Clean them up

`sudo vi /var/db/dhcpd_leases`

### Multipass

```sh
# List machines
multipass list
# shell in to a machine
multipass shell master-0
```

### Cannot connect to socket on macOS
close multipass GUI if it's open - this'll fix it either way

```bash
sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist
sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist
```
