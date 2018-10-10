# Finnaly, we can use Kubernetes...
Depending on the install method, current version or environment of your install, Kubernetes is a different beast to tame. The additition of (default) RBAC and not yet updated documentation can make you feel like Alice falling into a rabbit-hole. Best to keep focus on what you want to achieve, how and,when shared with others, if it will be sustainable and maintainable for others.


## Are we there yet..?
First we'll outline the different networtk and authentication methods we'll be using:
- For networking and network (policy) Canal (Calico/Flannel) was implement when following the previous <Setting up>-part
- Connecting and authenticating, amdministrative access

## Proxy access
Kubernetes is build up upon abstractions, which in theory make it more fit for purpose, abstracting some of the cluster complexity away from the 'normal' user. Depending on what you want to achieve with your Kubernetes setup there are overwhelming amounts of choices to make, depending on the tooling you want to use at an higher abstraction, some choices are dictated, simply because the tooling is opiniated or simply not ready to make use of yet another change to the Kubernets default install, let alone all dialects of Kubernetes provided by different vendors.

Basic proxy distiction can be made:
- cluster adminstration (e.g. kubectl proxy) and cluster administration API proxies
- pod/cluster networking proxies
- user proxies, publishing proxies


###Kubectl proxy for Dashboard access
First of we'll setup the dashboard for remote access. Since the introduction of RBAC (default on in recent versions of kubeadm), the Dashboard has some rights restricted. This results in a lot of access deniad banners. To give a correct overview of your cluster in the Dashboard you have to login using an account with the correct priviledges (or implement the non-recommended "give Dashboard service the rights to access cluster resources"-way).


## Ingress, NodePort, LoadBalancer, HostPort etc..
Publishing anything on Kubernetes is done via a Service, which gets coupled to a cluster resources like Pods (or also Service outside the cluster), to abstract away finding an available and correct Pod on Worker Nodes. 
Ingress pre works with HTTP, con works ONLY with HTTP
You need a controller I've selected the ingress-nginx kubernetes controller, which will act like a reverse proxy, getting requests to my Service and in turn to the correct Pod(s).
Other options: 
NodePort, mapping a (random) port on the Master which makes the Pod accesible on multiple Worker Nodes. For quick setup and demo an easy way to gain access to your published Pod 


## Integration strategy
