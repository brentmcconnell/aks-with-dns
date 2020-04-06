### AKS with ExternalDNS and CertManager
---

### Problem Statement

@ul[list-spaced-bullets text-09]
- How do we manage custom DNS entries for Ingress hostnames in Kubernetes
    - foo.example.com
    - bar.example.com
- For hosts that require an SSL certificate how can we manage certificates automatically from Kubernetes

@ulend

---
### Ingress Code Block
@snap[north-west span-100 text-05 text-gray]
Ingress Code Block
@snapend

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: nginx
  annotations:
    nginx.ingress.kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - nginx.designingdevops.com
    secretName: tls-secret
  rules:
  - host: nginx.designingdevops.com
    http:
      paths:
      - backend:
          serviceName: nginx-svc
          servicePort: 80
        path: /
```
@[11,14]

---
### Four Distinct Networking Problems in Kubernetes
@ul[list-fade-fragments]
- Container-to-Container 
- Pod-to-Pod
- Pod-to-Service
- Internet-to-Service
@ulend
---

### Internet-to-Service
@ul[list-spaced-bullets text-08]
- Ingress
    - When creating a Service we can optionally create a @css[text-uppercase](LoadBalancer) type 
    - Implemention of Load Balancer is provide by cloud controller (Azure)
    - Load Balancer gets traffic to AKS nodes where iptables takes over
    - Ingress Controller watches for Ingress resources and creates mappings to @css[text-uppercase](services)
- At this point we have traffic but how to we manage name resolution and SSL certificates for those names
    
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

### Setup CertManager
@ul[list-spaced-bullets text-08]
- Install NGINX if not already present
- Deploy CertManager to Kubernetes
- Create Issuer or ClusterIssuer objects 
    - Staging
    - Production
- Create Ingress object with reference to Issuer and host defined
@ulend
---    

# The END!
