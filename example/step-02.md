# Step 2

## Step 2 detail

### Check Syslog

```sh
vagrant ssh k8s-m-1

tail -d /var/log/syslog

^C to stop
```

## Administer the Kubernetes Cluster from your host

```sh
sudo apt-get update && sudo apt-get install -y apt-transport-https
```

Copy the Kubernetes config to your local home .kube dir

```sh
mkdir -p ~/.kube

vagrant port k8s-m-1

scp -P 2222 vagrant@127.0.0.1:/home/vagrant/.kube/config ~/.kube/config
```

List the Kubernetes cluster nodes using kubectl from your development host:

```sh
kubectl cluster-info

kubectl get nodes --all-namespaces

kubectl get pods --all-namespaces
```

## Install the Kubernetes Dashboard

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml

kubectl -n kubernetes-dashboard describe service kubernetes-dashboard
```

Apply the kubernetes-dashboard-service-np.yaml

```sh
kubectl apply -f kubernetes-dashboard-service-np.yaml
```

Obtain an authentication token to use on the Kubernetes Dashboard authentication realm

```sh
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

...
token:   <token>
```

Access the Kubernetes Dashboard using the URL [https://192.168.50.11:30002/#/login](https://192.168.50.11:30002/#/login) using the token printed before:

## Useful Vagrant commands

```sh
#Create the cluster or start the cluster after a host reboot
vagrant up

#Execute again the Ansible playlist in all the vagrant boxes, useful during development of Ansible playbooks
vagrant provision

#Execute again the Ansible playlist in the Kubernetes node 1
vagrant provision k8s-n-1

#Poweroff the Kubernetes Cluster
vagrant halt

#Open an ssh connection to the Kubernetes master
vagrant ssh k8s-m-1

#Open an ssh connection to the Kubernetes node 1
vagrant ssh k8s-n-1

#Open an ssh connection to the Kubernetes node 2
vagrant ssh k8s-n-2

#Stop all Vagrant machines (use vagrant up to start)
vagrant halt
```

## Next step

* [Step 3](step-03.md)
