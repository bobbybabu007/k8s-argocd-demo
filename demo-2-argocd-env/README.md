## ArgoCD CLI Quick Glossary
```bash
argocd
  account
  app
    create/delete
    diff
    edit
    history
    patch
    patch-resource
    sync
    set/unset
    rollback
    get
    wait
  cert
  cluster
  context
  login/logout/relogin
  repo
  repocreds
  version
```
</br>

## Commonly used commands
```bash
argocd login [argocdserver-svc-ip:port] --insecure
argocd project list
argocd cluster list
argocd repo list
argocd app create demo --repo [] --path [] --dest-namespace [] --dest-server []
argocd app list
argocd app sync demo
```

## ArgoCD CLI Demos

#### Create directory app
```bash
argocd app create demo \
  --repo=https://github.com/argoproj/argocd-example-apps.git
  --path=guestbook
  --dest-namespace default
  --dest-server https://kubernetes.default.svc
  --directory-recurse
```
</br>

#### Create a jsonnet app
```bash
argocd app create demo \
  --repo https://github.com/argoproj/argocd-example-apps.git
  --path jsonnet-guestbook
  --dest-namespace
  --dest-server https://kubernetes.default.svc
  --jsonnet-ext-str replicas=2
```
</br>

#### Create a Helm app from GitHub repo
```bash
argocd app create demo \
  --repo=https://github.com/argoproj/argocd-example-apps.git
  --path=helm-guestbook
  --dest-namespace default
  --dest-server https://kubernetes.default.svc
  --helm-set replicas=2
```
</br>

#### Create a Helm app from Helm repo
```bash
argocd app create demo \
  --repo https://kubernetes-charts.storage.googleapis.com
  --helm-chart nginx-ingress
  --revision 1.24.3
  --dest-namespace default
  --dest-server https://kubernetes.default.svc
```
</br>

#### Create a kustomize app
```bash
argocd app create demo \
  --repo=https://github.com/argoproj/argocd-example-apps.git
  --path=kustomize-guestbook
  --kustomize-image=gcr.io/heptio-images/ks-guestbook-demo=0.1
  --dest-namespace default
  --dest-server https://kubernetes.default.svc
```
</br>

#### Create an app using a custom tool
```bash
argocd app create demo \
  --repo=https://kubernetes-charts.storage.googleapis.com
  --path= plugins/kasane
  --dest-namespace default
  --dest-server https://kubernetes.default.svc
  --config-management-plugin kasane
```
