# 🛍️ Online Boutique – Microservices E-commerce Platform (Deployed on AWS EKS with HTTPS Ingress)

Online Boutique is a cloud-native microservices demo application composed of **11 microservices** written in Go, Java, Python, Node.js, and C#.  
This deployment version is fully production-ready and runs on **Amazon EKS** with:

- NGINX Ingress Controller  
- Let’s Encrypt DNS01 TLS (Cloudflare)  
- Helm umbrella chart  
- Kubernetes-native scaling and service discovery  


---

## 🧱 Architecture Overview

Online Boutique consists of **11 decoupled microservices**, each performing a single business function :

| Service | Language | Description |
|--------|----------|-------------|
| **frontend** | Go | Serves the website over HTTP & interacts with all services |
| **cartservice** | C# (.NET) | Stores/loads cart items in Redis |
| **productcatalogservice** | Go | Returns product catalog & search |
| **currencyservice** | Node.js | Converts prices using real ECB rates |
| **paymentservice** | Node.js | Mock credit card processor |
| **shippingservice** | Go | Calculates shipping rates & tracking IDs |
| **emailservice** | Python | Sends confirmation emails (mock) |
| **checkoutservice** | Go | Orchestrates the checkout workflow |
| **recommendationservice** | Python | Suggests products |
| **adservice** | Java | Context-based ads |
| **loadgenerator** | Python | Sends traffic for testing |

---

## 🛠 Technologies Used

| Components | Tools |
|-----------|-------|
| Cloud Platform | AWS EKS |
| Ingress | NGINX Ingress Controller |
| TLS | cert-manager + Let’s Encrypt DNS01 |
| Deployment | Helm Umbrella Chart |
| Networking | LoadBalancer + gRPC |
| Programming | Go, Python, Java, Node.js, C# |
| Observability | Ready for Prometheus, Grafana |

---

## 📁 Helm Umbrella Chart Structure

```
online-boutique/
│── Chart.yaml
│── values.yaml
│── templates/
│    ├── adservice.yaml
│    ├── cartservice.yaml
│    ├── checkoutservice.yaml
│    ├── productcatalogservice.yaml
│    ├── currencyservice.yaml
│    ├── paymentservice.yaml
│    ├── shippingservice.yaml
│    ├── emailservice.yaml
│    ├── recommendationservice.yaml
│    ├── shoppingassistant.yaml
│    ├── redis.yaml
│    ├── frontend.yaml
│    ├── ingress.yaml
│    
```

---

## 🔐 HTTPS With Let’s Encrypt (DNS01 Cloudflare)

### **ClusterIssuer (cert-manager)**  
`templates/clusterissuer.yaml`

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-dns01
spec:
  acme:
    email: "shivamsharmash03@gmail.com"
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-dns01-key
    solvers:
      - dns01:
          cloudflare:
            email: "shivamsharmash03@gmail.com"
            apiTokenSecretRef:
              name: cloudflare-api-token-secret
              key: api-token
```



## 🚀 Deploying on AWS EKS

### 1️⃣ Create EKS Cluster

```bash
eksctl create cluster \
  --name online-boutique \
  --region ap-south-1 \
  --nodes 3 \
  --node-type t3.medium
```

---

### 2️⃣ Install NGINX Ingress controller

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace
```

---

### 3️⃣ Install cert-manager

```bash
helm repo add jetstack https://charts.jetstack.io
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true
```

---

### 4️⃣ Create Cloudflare Secret

```bash
kubectl create secret generic cloudflare-api-token-secret \
  --from-literal=api-token=YOUR_TOKEN \
  -n cert-manager
```

---

### 5️⃣ Deploy Online Boutique Using Helm

```bash
helm install online . -n online-boutique --create-namespace
```

---

## 🎯 Final Result

✔ Fully deployed Online Boutique on AWS EKS  
✔ Secured with HTTPS using Let's Encrypt  
✔ Production-grade microservices architecture  
✔ NGINX Ingress routing through TLS  

---

## 👨‍💻 Author

**Shivam Sharma**  
DevOps Engineer | Kubernetes | Cloud | CI/CD | Infrastructure as Code
