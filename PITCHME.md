### AKS with ExternalDNS and CertManager

---

@ul[list-spaced-bullets text-07]
### Barebones Overview
- Kubernetes orchestrates containers
- Namespace is a virtual space inside Kubernetes
- Pod is a set of containers working together
- A Service is a way to expose pods to the cluster or world
- Ingress is how traffic can get to the pods from outside the cluster
@ulend

---

### Four Distinct Networking Problems in Kubernetes
@ul[list-fade-fragments]
- Container-to-Container 
- Pod-to-Pod
- Pod-to-Service
- Internet-to-Service
@ulend

---

### Container-to-Container
@ul[list-fade-fragments]
- A pod is a group of containers that share a network namespace
- Containers all have the same IP
- Containers can communicate over localhost
- Containers have access to shared volumes in pods
@ulend

---

### Pod-to-Pod
@ul[list-fade-fragments]
- All Pods have an IP address
- All Pods use that IP address to communicate... even across nodes
- All Pods have their own network namespace (ip netns list) on the host
- Same host pods communicate over a local bridge
- Routing Pod IPs to correct host is handled by container networking plugin
    - kubenet (default for AKS)
    - Azure Container Networking Interface (CNI)
@ulend
---

### Pod-to-Service
@ul[list-fade-fragments]
- Pod IPs are NOT durable
- Pod IPs will disappear and reappear in response to scaling, crashes or reboots
- @css[text-uppercase](Services) address this problem
    - Single IP representing a group of Pods
    - Pods can change over time and @css[text-uppercase](service) IP is persistent
    - Known as @css[text-blue](ClusterIP)
    - In cluster load balancer via iptables or IPV (IP Virtual Server)
    - @css[text-uppercase](Services) get internal DNS names (eg. my-svc.namespace.svc.cluster.local)
@ulend

---

### Internet-to-Service
@ul[list-fade-fragments]
- Egress
    - Pod IPs are SNAT'd to the VM's IP so that Load Balancer can route
- Ingress
    - When creating a Service can optionally create a @css[text-uppercase](LoadBalancer) type 
    - Implemention of Load Balancer is provide by cloud controller (Azure)
    - Load Balancer gets traffic to node where iptables takes over
    - Ingress Controller watches for Ingress resources and creates mappings to @css[text-uppercase](services)
    
@ulend
---

### Ingress Overview

![IMAGE](assets/img/ingress.png)

---

### Ingress Overview

![IMAGE](assets/img/ingress-obj.png)

---

@snap[north span-50 text-center]
### ExternalDNS
@snapend

@snap[west span-65]
@ul[list-spaced-bullets text-07]
- Allows control of DNS records dynamically from Kubernetes
- Will create and delete records based on Kubernetes state
- Supports all major cloud providers but at various levels.  Azure is in Beta.
- Uses txt records for awareness of records it is managing 

@ulend
@snapend

@snap[east span-35]
![IMAGE](assets/img/dns.png)
@snapend

---
@snap[north]
### Setup ExternalDNS
@snapend

@snap[west]
@ul[list-spaced-bullets text-07]
1. Create an Azure DNS Zone
1. Set nameservers correctly at Registrar
1. Create Service Principal and assign Contributor to DNS Zone
1. Modify existing NGINX service to include --publish-service option
1. Deploy ExternalDNS
@ulend
@snapend

---
@snap[north text-center]
### CertManager
@snapend

@snap[west span-65]
@ul[list-spaced-bullets text-07]
- Automate the management and issuance of TLS certificates
- Ensures certificates are valid and attempts to renew
- Self-signed, CA, Vault, Venafi, External, @css[txt-bold](ACME) 
- Automatic Certificate Management Environment (ACME) (eg. Let's Encrypt)
    - HTTP
    - DNS
@ulend
@snapend

@snap[east span-35]
![IMAGE](assets/img/cert.png)
@snapend
---

# The END!
