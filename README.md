<img width="439" height="74" alt="image" src="https://github.com/user-attachments/assets/0315f4de-58f7-42e1-9895-368f744359c1" /># traefik
Traefik is an Ingress Controller # it mean Traefik will handel/route the incoming Traffic as per the rules.
# it's the actual engine that reads routing rules and does this Layer 7 routing.
**internet/Client
      ▼
[External entry: LoadBalancer / NodePort / MetalLB VIP]
      ▼
[Traefik Pod(s)] ← runs as a Deployment, exposed via a Service
      │  reads routing rules from:
      │   - Kubernetes Ingress objects (standard), OR
      │   - Traefik CRDs: IngressRoute, Middleware (Traefik-native, more powerful)
      ▼
[Matches Host/Path rule] → e.g. app.example.com/api → routes here
      ▼
[Kubernetes Service (ClusterIP)] → e.g. mysql-app-svc
        ▼
[Backend Pod(s)]**

1. Install MEtal LB so that the traefik service will get loadbalancer IP if there is no loadbalancer then we can use the nodeport for the traefik service.
   $kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml # deploy the metalLB to so that we can use the Loadbalancer IP in traefik service
   # Metal LB will run as pod under the metal-lb namespace.
   kubectl get pods -n metallb-system
2. Create a file for configure the metal-lb-ip range, so that whenever a service will deploy as loadbalance the service will get the IP from this range.
   apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: production-pool
  namespace: metallb-system
spec:
  addresses:
    - 192.168.1.100-192.168.1.150
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2-advert
  namespace: metallb-system
spec:
  ipAddressPools:
    - production-pool
 # apply the kubectl -f apply metal-lb-iprange.yaml
 ### so basically it will create two component 1, L2Advertisement, IP pool range.
4. 
5. $ kubectl apply -f iprange.yaml   
   
# 3. create a namespace kubectl create namespace traefik-system
# 4. Step 3 — Set up RBAC (ServiceAccount, ClusterRole, ClusterRoleBinding)
  # 1. 

