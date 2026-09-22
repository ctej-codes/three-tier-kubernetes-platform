# three-tier-kubernetes-platform

A simple Kubernetes application with three tiers:

- **Web:** Nginx
- **API:** HashiCorp HTTP Echo
- **Data:** Redis with persistent storage

## Project Structure

```text
three-tier-kubernetes-platform/
├── 01-secret.yaml
├── 02-cache-pvc.yaml
├── 03-cache.yaml
├── 04-api.yaml
├── 05-web.yaml
└── kustomization.yaml
```

## Requirements

- Kubernetes cluster
- `kubectl`
- A default StorageClass for the Redis PVC

## Deploy

```bash
kubectl apply -k three-tier-kubernetes-platform/
kubectl get pods,services,pvc
```

The included Secret uses the placeholder value `replace-me`. Set a real token without committing it:

```bash
kubectl create secret generic api-secret \
  --from-literal=API_TOKEN="your-token-value" \
  --dry-run=client -o yaml | kubectl apply -f -
```

## Access the Services

Open the web application:

```bash
kubectl port-forward service/web 8080:80
```

Then visit <http://localhost:8080>.

Test the API:

```bash
kubectl port-forward service/api 8081:80
curl http://localhost:8081
```

Redis is available inside the cluster at `cache:6379`.

## Remove

```bash
kubectl delete -k three-tier-kubernetes-platform/
```

Do not commit real credentials to the repository. Kubernetes Secrets should be protected with encryption at rest or an external secret manager in production.
