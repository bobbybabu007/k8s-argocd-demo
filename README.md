#### ArgoCD Setup

```bash
# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# you can change and delete init password

```

#### Kustomize Setup

1. [Kustomize-GitHub](https://github.com/kubernetes-sigs/kustomize)
    >> Releases 
    >> [VERSION] 
    >> kustomize_v[VERSION]_darwin_amd64.tar.gz 
    => [DownloadURL]

2. Download & Install kustomize

```bash
# Download & Setup kustomize binary on Linux
cd /tmp
wget https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv4.5.5/kustomize_v4.5.5_darwin_amd64.tar.gz
tar xzf kustomize_v4.5.5_darwin_amd64.tar.gz
sudo mv kustomize /usr/local/bin

# Create kustomization.yaml in required directory
cd [k8s-directory-to-kustomize]
kustomize create
```
[Installation instructions](https://kubectl.docs.kubernetes.io/installation/kustomize/)
</br>

### Demo 1
Realise ArgoCD automation in same K8s cluster.

```bash
kubectl create -f https://raw.githubusercontent.com/bobbybabu007/k8s-argocd-kustomize/master/demo-1-argocd/application.yaml
```
</br>

### Demo 2
Realise ArgoCD automation in same K8s cluster.

```bash
kubectl create -f https://raw.githubusercontent.com/bobbybabu007/k8s-argocd-kustomize/master/demo-1-argocd/application.yaml
```
</br>

### Demo 3

1. Create kustomize directory structure & copy existing manifests

```bash
cd demo-3-kustomize
cp ./k8s/manifests .
```
</br>

2. Create kustomization.yaml in directory

```bash
kustomize create
nano kustomization.yaml
```
</br>


3. Kustomize your manifests using references 
from [kustomize file documentation](https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/)
</br>

4. Generate kustomized manifests for inspection before applying to the kubernetes cluster
```bash
kustomize build . > demo3app.yaml 
kubectl create -f demo3app.yaml
```
</br>

5. Alternatively, use kubectl create -k for kustomizing and creation on the fly
```bash
kubectl create -k demo3app.yaml
```
</br>

### Demo 4

1. Create kustomize directory structure & copy existing manifests

```bash
cd demo-4-kustomize-overlays
mkdir base
cp ./k8s/manifests/* .
mkdir overlays
```
</br>

2. Make directories for each env inside overlays

```bash
mkdir ./overlays/dev && \
mkdir ./overlays/uat && \
mkdir ./overlays/prod
```
</br>

3. Create kustomization.yaml in each env directory

```bash
kustomize create ./overlays/dev/ && \
kustomize create ./overlays/uat/ && \
kustomize create ./overlays/prod/
```
</br>

4. Kustomize your manifests using references 
from [kustomize file documentation](https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/) or using kustomize edit commands from [cmd documentation](https://kubectl.docs.kubernetes.io/references/kustomize/cmd/)

```bash
cd ./overlays/dev/
kustomize edit set namesuffix '-dev'

cd ../uat/
kustomize edit set namesuffix '-uat'

cd ../prod/
kustomize edit set namesuffix '-prod'
```
</br>

5. Generate kustomized manifests for inspection before applying to the kubernetes cluster

```bash
# Resources in DEV environment
kustomize build .\overlays\dev\ > .\overlays\dev\demo4app-sample-dev.yaml
kubectl create -f .\overlays\dev\demo4app-sample-dev.yaml

# Resources in UAT environment
kustomize build .\overlays\uat\ > .\overlays\uat\demo4app-sample-uat.yaml
kubectl create -f .\overlays\uat\demo4app-sample-uat.yaml

# Resources in PROD environment
kustomize build .\overlays\prod\ > .\overlays\prod\demo4app-sample-prod.yaml
kubectl create -f .\overlays\prod\demo4app-sample-prod.yaml
```
</br>

6. Alternatively, use kubectl create -k for kustomizing and creation on the fly
```bash
# Resources in DEV environment
kubectl create -k .\overlays\dev\

# Resources in UAT environment
kubectl create -k .\overlays\uat\

# Resources in PROD environment
kubectl create -k .\overlays\prod\
```
</br>

References
1. [Glossary - The Kustomization file](https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/)
2. [Glossary - Kustomize commands](https://kubectl.docs.kubernetes.io/references/kustomize/cmd/)
</br>

#### ArgoCD Documentation

</br>

#### Source

* Config repo: [https://gitlab.com/nanuchi/argocd-app-config](https://gitlab.com/nanuchi/argocd-app-config)

* Docker repo: [https://hub.docker.com/repository/docker/nanajanashia/argocd-app](https://hub.docker.com/repository/docker/nanajanashia/argocd-app)

* Install ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)

* Login to ArgoCD: [https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)

* ArgoCD Configuration: [https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/)
