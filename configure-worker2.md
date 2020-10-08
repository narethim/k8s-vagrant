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

deployment.yaml

```yml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: webapp1
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: webapp1
    spec:
      containers:
        name: webapp1
        image: katacoda/docker-http-server:latest
      ports:
        containerPort: 80
```

```sh
 kubectl apply -f deployment.yaml
```

service.yaml

```yml
apiVersion: v1
kind: Service
metadata:
   name: webapp1-svc
   labels:
     app: webapp1
spec:
   type: NodePort
   ports:
     port: 80
     nodePort: 30080
   selector:
     app: webapp1
```

```sh
 kubectl apply -f service.yaml
```
