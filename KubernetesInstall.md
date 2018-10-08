# Kubernetes running on local Virtualbox Ubuntu 18.04 LTS install 
Configure 1 master with at least 1GB of memory and 2 to 3 Worker nodes with each 4GB of memory. Configure them with Bridged network setup in Virtualbox, but do adjust the machines to have a fixed IP after install, make sure this fixed IP does not overlap with DHCP-scope or get a reserved part.

Make sure to update after install:

<code>sudo apt-get dist-update && sudo apt-get dist-upgrade -y</code>

## Docker install
Install Docker for the container runtime:

```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

sudo apt-get update

sudo apt-get install docker-ce

```

Then add your normal user to docker group and logout/login to let these changes take effect:

```
sudo usermod -aG docker ${USER}

```

## Prepare Kubernetes install (all nodes)
Check all nodes have unique MAC/IP-adresses and that all nodes can see each-other on the network. Make sure you have assigned fixed IP addresses.

### Static Network
In Ubuntu 18.04 they added the default netplan network manager, it uses yaml files to describe network components. 
Create a file: 
```
sudo touch /etc/netplan/50-cloud-init.yaml 
```
And edit it with your favorite editor to reflect your static setup, here is my file for my Master as reference, adjust to your own setup and adjust the adrdress for each cluster node, pay close attention to spaces and naming of network interfaces (enp0s3):
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
     addresses:
       - 192.168.0.231/24
     gateway4: 192.168.0.1
     nameservers:
       addresses: [192.168.0.1, 8.8.8.8, 8.8.4.4]
```
Then execute the following to apply:
```
sudo netplan apply
```
### Installing kubeadm, kubectl and kubelet
These steps were taken from the kuberetes.io website, setting up kubeadm
To run the commands as Root run:
```
sudo su
```
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```
The last command is there to not automatically upgrade Kubernets with a normal system update.


## Install Kubernetes Cluster Master
To install the Kubernetes Master login as Root by typing:
```
sudo su
```
and as Root type in the following command and press enter:
```
kubeadm init --pod-network-cidr=10.244.0.0/16
```

This will install Kubernetes on the Master node and initialize the network used for the cluster pod-network provider to work with the Canal provider (a combo of Calico for policy and Flannel for network).
Take note of the output to enable the worker nodes to join the cluster the part which starts with:
```
kubeadm join 192.168.0.231:6443 --token:.......................
```

Setup kubectl to use as normal user by adjusting the kubectl configuration:
```
 mkdir -p $HOME/.kube
 sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
 sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

To enable this pod-network type the following after setup is done:
```
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/canal/rbac.yaml
```
```
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/canal/canal.yaml
```
Then check if all the Kubernets pods are running:
```
kubectl get pods --all-namespaces
```

## Join the Worker Nodes to the Cluster (after setting them up according to Perpare Kubernetes install (all nodes))
Run the join command according to how it was provided to you by the setup on the Master again as Root (sudo su), after the setup logout and check (after a minute or so) if the worker node has joined the cluster by typing following command on the Master:
```
kubectl get nodes
```

Now repeat on the other cluster worker nodes!
