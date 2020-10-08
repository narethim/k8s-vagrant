# Configure `master` node

```sh
cd k8s-vagrant/worker1

vagrant up

vagrant ssh
```

#### Change hostname

```sh
sudo vi /etc/hosts
```

Replace `vagrant` with `master`

#### Disable swap

```sh
sudo swapoff -a

sudo vi /etc/fstab
```

Comment out the following:

```sh
/dev/mapper/vagrant--vg-root /               ext4    errors=remount-ro 0       1
#/dev/mapper/vagrant--vg-swap_1 none            swap    sw              0       0
```

#### Add the Docker Repository

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add –

sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

#### Add the Kubernetes Repository

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add –

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list

deb https://apt.kubernetes.io/ kubernetes-xenial main

EOF
```

#### Install

```sh
sudo apt-get update

# Install docker-ce
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Install k8s
sudo apt-get install -y kubelet kubeadm kubectl

# sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.12.2-00 kubeadm=1.12.2-00 kubectl=1.12.2-00

sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

#### Enable bridge networking

```sh
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p
```

#### Run following only on `master` node

```sh
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.56.30
```

This will keep  a token which help to join the other nodes. Please keep it save.

```sh
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

kubectl get nodes

(wait for sometime, it status will become ready)

```

