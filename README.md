# Kubernetes running on local Virtualbox Ubuntu 18.04 LTS install 
This repository describes my personal local setup of a Kubernetes cluster, following on-line documentation. I'll update this repository as I re-work through the setup ever so often for demo purposes. Current date is oktober 2018, goals at this point are:

- [x] Install and bootstrap cluster nodes [Kubernetes Install](doc/KubernetesInstall.md)
- [x] Install with Kubeadm [Kubernetes Install](doc/KubernetesInstall.md)
- [ ] Configure RBAC, AuthN/AuthZ for use of Kubernetes [Using Kubernetes](doc/UsingDeployingk8s.md)
- [ ] Configure Integration (CI/CD) ..... [CI/CD with Kubernetes](doc/Workflow.md)
- [ ] Actual Demo 

Do not underestimate the amount of time you'll spend making this setup work for you, most of the documentation is made for my own reference and to have a headstart when setting up a demo a few months down the road. To get to the final point in the install document I've had to rebuild the Cluster at least 5 times, thankfully kubeadm makes cleaning up and rebuidling easy enough..but when deploying more workloads and integration this process gets harder every time. On the other hand rebuilding often makes you very aware of the setup and the inner workings, which you tend to forget if you only follow a tutorial once (like this one ;-)).

Configure 1 master with at least 1GB of memory and 2 to 3 Worker nodes with each 4GB of memory. Configure them with Bridged network setup in Virtualbox, but do adjust the machines to have a fixed IP after install, make sure this fixed IP does not overlap with DHCP-scope or get a reserved part. In my case I have a small portable hub, which I can NAT through to my phones shared wifi for Internet connection during demo, your setup might vary..

# Demo Workflow Software Delivery, then Deploy to production..
Setup for this demo is based on gitlab.com online service working with the local kubernetes cluster install. In future I'll also setup a stand-a-lone gitlab instance or in a different kubernetes namespace on the cluster, not sure yet.. 
What is needed is AuthN/AuthZ setup in RBAC'ed Kubernetes cluster to work and to not have priveledge escalation possibilities in the Docker build process (which should be possible now that multiple solutions are available for not running as root and not depending on the docker runtime for build..)
<pre>
[DEV]--Checks-in-Code-->[CI][CD]-Deploy-to-Production-->[PROD]     
</pre>

For build repository for Docker, I'll select either the docker hub or the one in gitlab, not sure yet..
