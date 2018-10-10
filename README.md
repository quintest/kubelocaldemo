# Kubernetes running on local Virtualbox Ubuntu 18.04 LTS install 
This repository describes my personal local setup of a Kubernetes cluster, following on-line documentation. I'll update this repository as I re-work through the setup ever so often for demo purposes. Current date is oktober 2018, goals at this point are:


- [x] Install and bootstrap cluster nodes [Kubernetes Install](doc/KubernetesInstall.md)
- [x] Install with Kubeadm [Kubernetes Install](doc/KubernetesInstall.md)
- [ ] Configure RBAC, AuthN/AuthZ for use of Kubernetes [Using Kubernetes](doc/UsingDeployingk8s.md)
- [ ] Configure Integration (CI/CD) ..... [CI/CD with Kubernetes](doc/Workflow.md)
- [ ] Actual Demo 

Configure 1 master with at least 1GB of memory and 2 to 3 Worker nodes with each 4GB of memory. Configure them with Bridged network setup in Virtualbox, but do adjust the machines to have a fixed IP after install, make sure this fixed IP does not overlap with DHCP-scope or get a reserved part. In my case I have a small portable hub, which I can NAT through to my phones shared wifi for Internet connection during demo, your setup might vary..

# Demo Workflow Software Delivery, then Deploy to production..
Setup for this demo is based on gitlab.com online service, in future I'll also setup a stand-a-lone gitlab instance or in a different kubernetes namespace, not sure yet.. 
What is needed is AuthN/AuthZ setup in RBAC'ed Kubernetes cluster to work and to not have priveledge escalation possibilities in the Docker build process (which should be possible now that multiple solutions are available..)
<pre>
[DEV]--Checks-in-Code-->[CI][CD]-Deploy-to-Production-->[PROD]     
</pre>
