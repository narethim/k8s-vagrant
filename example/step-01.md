# Step 1

## Step 1 detail

### Clone git source

```sh
git clone https://github.com/ITWonderLab/ansible-vbox-vagrant-kubernetes.git

cd ansible-vbox-vagrant-kubernetes
```

### Create all the nodes

```sh
vagrant up
```

### Access nodes

Login to master node: k8s-m-1

```sh
vagrant ssh k8s-m-1
vagrant@k8s-m-1:~$
```

Login to node1: k8s-n-1

```sh
vagrant ssh k8s-n-1
vagrant@k8s-n-1:~$
```

Login to node2: k8s-n-2

```sh
vagrant ssh k8s-n-2
vagrant@k8s-n-2:~$
```

## Next step

* [Step 2](step-02.md)
