# Helm Package Management

**Helm** is the package manager for Kubernetes. It helps you manage Kubernetes applications through **Charts**, which are pre-configured packages of Kubernetes resources. With Helm, you can define, install, and upgrade even the most complex Kubernetes applications.

---

## Key Concepts

* **Chart**: A bundle of information that database-describes a set of Kubernetes resources. You can think of it as a package.
* **Repository**: A place where charts can be collected and shared.
* **Release**: A running instance of a chart in a Kubernetes cluster. You can install the same chart multiple times, and each install will be a new release (e.g., `dev-nginx`, `prod-nginx`).
* **Values**: Custom configuration options that overwrite the default settings in a Chart (typically defined in a `values.yaml` file).

---

## Essential Helm Commands

Use these commands to manage applications using Helm.

### 1. Repository Management

Before installing applications, you must register the charts' remote registries:

```bash
# Add a chart repository (e.g., Bitnami for common application stacks)
helm repo add bitnami https://charts.bitnami.com/bitnami

# Add the official Kubernetes community charts repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

# Update all registered repositories to retrieve the latest chart definitions
helm repo update

# List your added repositories
helm repo list
```

### 2. Search for Packages

Find charts across the web or in your added repositories:

```bash
# Search your local repositories for a package (e.g., nginx)
helm search repo nginx

# Search Artifact Hub (global internet package index)
helm search hub nginx
```

### 3. Install a Package (Release)

Install a chart as a release in a target namespace:

```bash
# Create namespace if it doesn't exist
kubectl create namespace web-apps

# Install Nginx from the Bitnami repo and name the release 'my-web-server'
helm install my-web-server bitnami/nginx --namespace web-apps
```

### 4. Customizing Deployments

Customize default configuration variables during installation:

#### Method A: Using `--set` (Inline)
```bash
helm install my-web-server bitnami/nginx \
  --set replicaCount=2 \
  --set service.type=NodePort \
  --namespace web-apps
```

#### Method B: Using a custom `custom-values.yaml` File (Recommended)
Create a file named `custom-values.yaml` with your custom configuration:
```yaml
replicaCount: 2
service:
  type: LoadBalancer
  ports:
    http: 8080
```
Deploy the chart using the file overrides:
```bash
helm install my-web-server bitnami/nginx -f custom-values.yaml --namespace web-apps
```

### 5. Upgrades and Rollbacks

Update configurations seamlessly without bringing down the service, or roll back if an issue occurs:

```bash
# Upgrade the release configuration using updated overrides
helm upgrade my-web-server bitnami/nginx -f custom-values.yaml --namespace web-apps

# Upgrade-or-install: Installs the chart if it doesn't exist, otherwise upgrades it
helm upgrade --install my-web-server bitnami/nginx -f custom-values.yaml --namespace web-apps

# Review release history (shows all revision numbers and descriptions)
helm history my-web-server --namespace web-apps

# Roll back the release to a previous revision (e.g., revision 1)
helm rollback my-web-server 1 --namespace web-apps
```

### 6. Inspecting Releases

Query status and configuration of installed software:

```bash
# List all active releases across all namespaces
helm list --all-namespaces

# View the status of a specific release (includes notes, pods, and services)
helm status my-web-server --namespace web-apps

# Fetch the exact user-supplied values used to configure the release
helm get values my-web-server --namespace web-apps
```

### 7. Uninstalling Releases

Remove the package and all its associated Kubernetes resources from the cluster:

```bash
helm uninstall my-web-server --namespace web-apps
```

---

## Best Practices

> [!TIP]
> Always use `helm upgrade --install` with custom value files checked into your Git repository. This conforms to GitOps patterns and ensures reproducible configuration configurations.

> [!WARNING]
> Do not put credentials, sensitive API keys, or private SSH keys directly in public `values.yaml` files. Instead, use an external secrets controller (like Sealed Secrets or HashiCorp Vault) or pass them securely during runtime execution.
