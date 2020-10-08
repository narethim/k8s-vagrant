# Configure `worker1` node

```sh
cd k8s-vagrant/worker1

vagrant up

vagrant ssh
```

## Change hostname from `vagrant` to `worker1`

```sh
sudo vi /etc/hosts
```

Replace `vagrant` with `worker1`

## Disable swap on `worker1` node

```sh
sudo swapoff -a

sudo vi /etc/fstab
```

Comment out the following:

```sh
/dev/mapper/vagrant--vg-root /               ext4    errors=remount-ro 0       1
#/dev/mapper/vagrant--vg-swap_1 none            swap    sw              0       0
```

## Add the Docker Repository to node `worker1`

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg > docker-key
sudo apt-key add docker-key
sudo rm docker-key

sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

## Add the Kubernetes Repository to node `worker1`

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg > k8s-key
sudo apt-key add k8s-key
sudo rm k8s-key

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

## Install docker on node `worker1`

```sh
sudo apt-get update

sudo apt-get install -y docker-ce kubelet kubeadm kubectl

sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

## Enable bridge networking on node `worker1`

```sh
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p
```
