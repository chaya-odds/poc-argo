# Argo CD

[Argo CD](https://argo-cd.readthedocs.io/en/stable/) is a GitOps tool that automates the deployment and management of applications and tools on Kubernetes using Git repositories. For the `ops-cluster`, Argo CD is configured to run in High Availability (HA) mode and uses [Helm](https://helm.sh/) for easy installation, upgrades, and uninstallation.

If you need to adjust the configuration, please modify the [values.yaml](values.yaml) file.

### Installing

Add the Argo Helm repository

```sh
helm repo add argo https://argoproj.github.io/argo-helm
```

Install Argo CD using Helm

```sh
helm install argo-cd argo/argo-cd --version 7.1.1 --values values.yaml --namespace argo-cd --create-namespace
```

After a successful installation, ensure you have created a [Keycloak client](https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/keycloak/#creating-a-new-client-in-keycloak), then set up Keycloak by running the following command:

```sh
OIDC_KEYCLOAK_CLIENT_SECRET="your-client-secret"
kubectl -n argo-cd patch secret argocd-secret -p='{"data":{"oidc.keycloak.clientSecret": "'$(echo -n $OIDC_KEYCLOAK_CLIENT_SECRET | base64)'"}}'
```

Replace `your-client-secret` with your actual [Keycloak client secret](https://sso.odd.works/admin/master/console/#/odds/clients/a1901f5d-cfe6-41fc-bad5-cbc1dc57ccd8/credentials)

Next, change the in-cluster name to `ops` for ease of use

### Upgrading

```sh
helm upgrade argo-cd argo/argo-cd --version 7.1.1 --values values.yaml --namespace argo-cd
```

### Uninstalling

```sh
helm uninstall argo-cd argo/argo-cd --namespace argo-cd
```
