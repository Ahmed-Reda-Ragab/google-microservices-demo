Local cluster setup (kind + ingress) for this project

1. Install kind and kubectl
   - https://kind.sigs.k8s.io/
   - https://kubernetes.io/docs/tasks/tools/

2. Create kind cluster with custom config (optional)
   - `kind create cluster --name mycluster --config kind-config.yaml`
   - If you don’t have a config file, use `kind create cluster --name mycluster`.

3. Install ingress-nginx in cluster
   - `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml`

4. Verify nodeports/ingress controller is ready
   - `kubectl -n ingress-nginx get pods`
   - `kubectl get svc -n ingress-nginx`

5. Build project services in Docker and load into kind
   - Example service: `docker build -t gcp-services-currencyservice ./src/currencyservice`
   - `kind load docker-image gcp-services-currencyservice --name mycluster`

6. Apply Kubernetes manifests
   - `kubectl apply -f k8s/`

7. Verify everything is running
   - `kubectl get pods --all-namespaces`
   - `kubectl get svc --all-namespaces`

8. Access frontend via ingress
   - Find ingress host: `kubectl get ingress -n frontend`
   - Add host to `/etc/hosts` if needed (e.g., `127.0.0.1 frontend.local`)

Notes
- kind does not include a cloud load balancer by default; you can use `metallb` if service type LoadBalancer is required.
- Replace image names with the service(s) you are testing.

