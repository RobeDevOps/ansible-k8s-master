robedevops.ansible_k8s_master
=========

* Start the master node for k8s cluster.
* Copy the cluster configuration under /home/centos/.kube directory (still working for other environments)
* Verify Master node is ready after the kubeadm init.


Role Variables
--------------

N/A

Dependencies
------------

It dependes on role [**robedevops.ansible_k8s_nodes**](https://github.com/RobeDevOps/ansible-k8s-nodes)

Example Inventory
-----------------

```bash
master ansible_host=host_ip_address
worker1 ansible_host=hos_ip_address
worker2 ansible_host=host_ip_address

[k8s-nodes]
master
worker1
worker2

[k8s-masters]
master

[k8s-workers]
worker1
worker2

[all:vars]
ansible_connection=ssh
ansible_user=centos
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
ansible_ssh_private_key_file=/home/path/to/demo.pem
```

Example Playbook
----------------

```yaml

- hosts: k8s-nodes
  gather_facts: yes
  become: yes
  roles:
    - { role: robedevops.ansible_docker, tags: ['docker'] }
    - { role: robedevops.ansible_docker_user, tags: ['docker-user'] }
    - { role: robedevops.ansible_k8s_nodes, tags: ['k8s-nodes'] }

# previous roles install and configure all the required tools to start a k8s manager node
- hosts: k8s-masters
  gather_facts: yes
  become: yes
  roles:
    - { role: robedevops.ansible_k8s_master, tags: ['k8s-master'] }
```

License
-------

BSD

Author Information
------------------

Roberto Cardenas Isla - email: rcardenas20@gmail.com
