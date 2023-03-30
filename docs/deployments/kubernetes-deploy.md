# Kubernetes Cluster 

!!! success "Prerequisites"

    * Make sure you have at least **6** VMs configured with Centos7

        * 1 Master node
        
        * 4 worker nodes
        
        * 1 worker node dedicated only for **vice-apps**

    *  Make sure you have Ansible installed, and you can reach your VMs via `ssh`


# TODO rewrite the docs once [ansible-k8s-centos7](https://github.com/cyverse-austria/ansible-k8s-centos7) is ready.

We are using **Ansible** to set up a kubernetes cluster.

Current kubernetes cluster is configured using ansible playbooks from [deployments/ansible/kubernetes](https://github.com/cyverse-de/deployments/tree/main/ansible/kubernetes).

# Looking to setup your own k8s cluster?

If your are intrested to setup your own kubernetes cluster on Centos 7, please follow the steps bellow.


## Steps

### Clone the repo

```bash
# clone repo
git clone https://github.com/cyverse-de/deployments.git

# navigate to ansible playbooks for k8s
cd /ansible/kubernetes

```

### create your inventory

Create your inventory file under `/inventory/cyverse` and replace the host names as yours.
```bash
[k8s:children]
k8s-control-plane
k8s-worker

[kube-apiserver-haproxy]
k8s-reverse-proxy.example.com

[k8s-control-plane]
k8s-c1.example.com

[k8s-storage:children]
k8s-worker

[k8s-worker]
k8s-w1.example.com
k8s-w2.example.com
k8s-w3.example.com
k8s-w4.example.com
vice-w1.example.com


[outward-facing-proxy]
vice-haproxy.example.com

[haproxy]
vice-haproxy.example.com

[vice-workers]
vice-w1.example.com
```

### Run playbooks
**Note:** we are using `--user root`, if your virtual machines have a diffrent user you could change this.

#### Check if your hosts are reachable

```bash
ansible -i inventory/ -m ping all --user root
```

#### Setup firewall configs

```bash
ansible-playbook -i inventory/ firewalld-config.yml --user root
```

#### Provision your nodes

```bash
ansible-playbook -i inventory/ provision-nodes.yml --user root
```

#### Tainting and Labeling VICE Worker Nodes

```bash
kubectl label nodes vice-w1.example.com vice=true
kubectl taint nodes vice-w1.example.com vice=only:NoSchedule
```
