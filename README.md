# Mini Kubernetes Environment

This repository contains a **mini Kubernetes environment** for learning and experimenting with DevOps concepts on Minikube.  
It includes a frontend and backend microservice, internal services, and an ingress for external access.

---

## Structure

### Namespaces
- All resources are under the `dev` namespace.

### Frontend
- **Deployment**: Runs `nginx:alpine` with a ConfigMap-mounted HTML page.
- **Service**: ClusterIP service to expose frontend to other services.
- **ConfigMap**: Contains `index.html` displayed by NGINX.

### Backend
- **Deployment**: Runs `hashicorp/http-echo` to simulate a simple backend.
- **Service**: ClusterIP service exposing the backend on port 80.

### Ingress
- Routes requests from `frontend.local` to the frontend service.
- Requires `minikube tunnel` and a `/etc/hosts` entry for access.

---

## How to Use

1. **Start Minikube** with your preferred profile:
```bash
minikube start -p dev-cluster
```

2. **Apply the resources:**
```bash
kubectl apply -f dev/
```

3. **Start the ingress tunnel**
```bash
sudo minikube tunnel -p dev-cluster
```

4. **Add hosts entry" (for mac)
```bash
echo "127.0.0.1 frontend.local" | sudo tee -a /etc/hosts
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```

5. ** Test the Frontend **
* Open "http://frontend.local" in your broweser.
Or:
```bash
curl -H "Host: frontend.local" http://127.0.0.1
```

-----------------
Purpose

This layout simulates a real-life DevOps environment with:
* Deployments & replicas
* ConfigMaps for configuration
* Services for internal communication
* Ingress for external routing

It’s ready to be extended for CI/CD integration.


