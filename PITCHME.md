### AKS with ExternalDNS and CertManager

---

### Background Info To Know
@ul[list-fade-fragments]
- Basic Kubernetes Knowledge
    - Kubectl
    - Helm
    - Load Balancer
- Let's Encrypt Basics
@ulend

---

### Barebones Overview
- Kubernetes orchestrates containers
- Namespace is a virtual space inside Kubernetes
- Pod is a set of containers working together
- A Service is a way to expose pods to the cluster or world
- Ingress is how traffic can get to the pods from outside the cluster

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
@ulend



---

### Ingress Overview

![IMAGE](assets/img/ingress.png)

---

@snap[north span-50 text-center]
### ExternalDNS
@snapend

@snap[west span-55]
@ul[list-spaced-bullets text-09]
- You will be amazed
- What you can achieve
- With a **little imagination**
- And GitPitch Markdown
@ulend
@snapend

@snap[east span-45]
![IMAGE](assets/img/dns.png)
@snapend

---

@snap[north span-50 text-center]
### CertManager
@snapend

@snap[west span-55]
@ul[list-spaced-bullets text-09]
- You will be amazed
- What you can achieve
- With a **little imagination**
- And GitPitch Markdown
@ulend
@snapend

@snap[east span-45]
![IMAGE](assets/img/cert.png)
@snapend
---

# The END!
