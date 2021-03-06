# Kubernetes Hard Way Ansible
Ansible template to create kubernetes cluster with the following specs:
* Kubernetes 1.19.10
* Flannel v1.13.0
* CNI v0.9.1
* CRI v1.19.0
* runc v1.0.0-rc93
* containerd v1.4.4
* etcd v3.4.15
* core-dns 1.7.1
* metrics-server v0.4.3
* Haproxy & keepalived
* Secure communication between component

### Additional Feature
* OIDC supported
* MetalLB supported
* Vagrant installation supported
* Insecure Registry supported
* Renewing certificate playbook
* Adding worker node playbook
* Upgrade Kubernetes playbook

### Tested Environment
* Ubuntu 18.04
    * 3 master nodes, 3 worker nodes
    * 3 etcd nodes, 3 master nodes, 3 worker nodes

## Step Installation
Execution happen on the deployer node. All the ceritificate generated and store in the deployer node. The deployer node cannot be deleted if you want to renew certificate or extending kubernetes worker node. All this step executed in the deployer node.

* Prepare ansible
```bash
sudo apt-add-repository ppa:ansible/ansible -y
sudo apt update
sudo apt install ansible -y
```
* Make sure have access into all nodes

please make sure that <user> have privilege access, you can add the user in sudoers files, after bootstrap is done, fell free to remove that.
```bash
ssh-keygen

# copy to deployer itself
ssh-copy-id <user>@<deployer-node>

# copy to etcd node
ssh-copy-id <user>@<etcd-node>
ssh-copy-id <user>@<etcd-node>
ssh-copy-id <user>@<etcd-node>

# copy to master node
ssh-copy-id <user>@<master-node>
ssh-copy-id <user>@<master-node>
ssh-copy-id <user>@<master-node>

# copy to master node
ssh-copy-id <user>@<worker-node>
ssh-copy-id <user>@<worker-node>
ssh-copy-id <user>@<worker-node>
```

* disable ansible hostkey checking
```bash
vi ~/.ansible.cfg

[defaults]
host_key_checking = False
```

* Clone this repository
```bash
git clone https://github.com/zufardhiyaulhaq/kubernetes-hardway-ansible.git
git checkout --track origin/<TAG>
```

* Adjust variable in the group_vars
```
vi group_vars/all.yml
```

* Adjust Kubernetes host and nodes
```
vi hosts/hosts
```

* Run ansible
```
ansible-playbook main.yml -i hosts/hosts
```

Please backup certificate directory in the deployer node!

### Additional Setup
* [OIDC](additional_setup/oidc.md)
* [metalLB](additional_setup/metallb.md)
* [Private Insecure Registry](additional_setup/insecure-registry.md)
* [Renew Certificate](renew-certificate.md)
* [Add new Worker](new-worker.md)
* [Upgrade Kubernetes](upgrade-kubernetes.md)
