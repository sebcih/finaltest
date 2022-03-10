# How to Install

1. Make sure that [Helm](https://helm.sh/docs/intro/install/) is installed.

2. In charts/argo-cd build dependencies 
```
helm dependency build
```

3. Install argo with helm
 ```
helm install argo-cd . --create-namespace --namespace argo -f values.yaml --debug
```
4. Get initial admin password 
```
kubectl -n argo get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
5. Apply the secret so we can reach our repo
```
kubectl apply -f argo-repo-secret.yaml
```

6. Create an  application (root) to serve as an app-of-apps
```
helm template apps/ | kubectl apply -f -
```

7. Now we can delete argo from helm
``` 
kubectl delete secret -l owner=helm,name=argo-cd
```
