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
- Container-to-Container 
- Pod-to-Pod
- Pod-to-Service
- Internet-to-Service

@[4]

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
