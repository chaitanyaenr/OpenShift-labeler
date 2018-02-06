# openshift-labeler
Tool to label openshift nodes based on the role they play in openshift cluster and generate a tooling inventory on fly by looking at the node labels.

### Requirements
- Ansible
- Inventory used to install openshift

### Run
```
$ cd openshift-labeler
$ ansible-playbook -vv -i <openshift-inventory> openshift_label.yml
```

### Labeling of openshift nodes
The nodes in the openshift cluster are labeled as follows:

- Master and etcd are co-located - role=master_etcd, type=pbench
- Master                         - role=master, type=pbench
- Nodes                          - role=node, type=pbench
- Etcd                           - role=etcd, type=pbench
- lb                             - role=lb, type=pbench
- glusterfs                      - role=cns, type=pbench

### Sample Inventory generated
```
[pbench-controller]
foo.controller.com


[masters]
foo.master.com

[nodes]
foo.node.com

[etcd]
foo.master.com

[lb]
foo.lb.com

[glusterfs]
foo.cns.com

[prometheus-metrics]
foo.master.com port=8443 cert=/etc/origin/master/admin.crt key=/etc/origin/master/admin.key
foo.master.com port=10250 cert=/etc/origin/master/admin.crt key=/etc/origin/master/admin.key
foo.node.com port=10250 cert=/etc/origin/master/admin.crt key=/etc/origin/master/admin.key
foo.node.com port=10250 cert=/etc/origin/master/admin.crt key=/etc/origin/master/admin.key

[pbench-controller:vars]
register_all_nodes=False
```

## Location of the inventory
By default it genrates the inventory at /root/tooling_inventory, inv_path variable can be set to a different path to change the location.
