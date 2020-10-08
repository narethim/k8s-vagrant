# Configure `worker2` node

```sh
cd k8s-vagrant/worker2

vagrant up

vagrant ssh
```

## Change hostname from `vagrant` to `worker2`

```sh
sudo vi /etc/hosts
```

Replace `vagrant` with `worker2`

## Disable swap on `worker2` node

```sh
sudo swapoff -a

sudo vi /etc/fstab
```

Comment out the following:

```sh
/dev/mapper/vagrant--vg-root /               ext4    errors=remount-ro 0       1
#/dev/mapper/vagrant--vg-swap_1 none            swap    sw              0       0
```

## Add the Docker Repository to node `worker2`

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg > docker-key
sudo apt-key add docker-key
sudo rm docker-key

sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

## Add the Kubernetes Repository to node `worker2`

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg > k8s-key
sudo apt-key add k8s-key
sudo rm k8s-key

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

## Install docker on node `worker2`

```sh
sudo apt-get update

sudo apt-get install -y docker-ce kubelet kubeadm kubectl

sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

## Enable bridge networking on node `worker2`

```sh
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p
```

## Join master

```sh
# Join master node
sudo kubeadm join <master_ip>:6443 –token <token> --discovery-token-ca-cert-hash <hash>
```

Example:

```sh
# Join master node
sudo kubeadm join 192.168.56.30:6443 --token en93zd.xmk7vrkwr6taksmu \
    --discovery-token-ca-cert-hash sha256:9b3683b18ddcf8c7cc161aa2adc32d7a18171c8bf983091dbfb6c9e727acfe43
```
