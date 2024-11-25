# Argo Apps

Argo Apps simplifies the management of additional Argo CD Applications and Projects.

If you need to adjust Argo CD `Applications` or `Projects`, please modify the [values.yaml](values.yaml) file.

### Installing

Add the Argo Helm repository

```sh
helm repo add argo https://argoproj.github.io/argo-helm
```

Install Argo Apps using Helm

```sh
helm install argo-apps argo/argocd-apps --version 2.0.0 --values values.yaml --namespace argo-cd
```

### Upgrading

```sh
helm upgrade argo-apps argo/argocd-apps --version 2.0.0 --values values.yaml --namespace argo-cd
```

### Uninstalling

```sh
helm uninstall argo-apps argo/argocd-apps --namespace argo-cd
```
