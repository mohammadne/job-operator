# job-operator

> the operator will manage the combination of all components as one unit
>
> here we aimed to create an an operator which enables to run jobs on the cluster (with custom logging) and log pod lifecycle changes

## installation

```sh
# setup local kind cluster
kubectl get nodes
kubectl auth can-i create deployments
kubectl auth can-i list secrets

# install kubebuilder
kubebuilder version

# initialize project (golang)
kubebuilder init --project-name job-operator --domain=example.com --repo=github.com/mohammadne/job-operator
tree -L 1

kubebuilder edit --multigroup=true

# scaffold CRD's API
kubebuilder create api --group=job --version=v1alpha1 --kind=At --controller --resource
```

## usage

```bash
# install prerequisites

# clone the repository

make install

make deploy

# update the api/v1alpha1/zz_generated.deepcopy.go
make generate

# generate the CRD manifests at config/crd/bases/app.example.com_podsets.yaml
make manifest
```

## resources

- [Writing a kubernetes controller in Go with kubebuilder](https://dev.to/ishankhare07/writing-a-simple-kubernetes-controller-in-go-with-kubebuilder-ib8)

- [What is a Kubernetes Operator?](https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/)

- [Demo Memcached Operator using Operator SDK](https://www.youtube.com/watch?v=9QR3sRp-6Xk&ab_channel=AustinMacdonald)

- [Build and deploy a basic operator](https://developer.ibm.com/learningpaths/kubernetes-operators/develop-deploy-simple-operator/create-operator/) and [Explanation of Memcached operator code](https://developer.ibm.com/learningpaths/kubernetes-operators/develop-deploy-simple-operator/deep-dive-memcached-operator-code/)


## Running

``` bash
# scaffold project
operator-sdk init --project-name podset-operator --domain=example.com --repo=github.com/mohammadne/sandbox/podset-operator

# https://book.kubebuilder.io/migration/multi-group.html
kubebuilder edit --multigroup=true

# create api
operator-sdk create api --group=app --version=v1alpha1 --kind=PodSet --controller --resource

# make changes to the api
vim api/v1alpha1/podset_type.go

# update the api/v1alpha1/zz_generated.deepcopy.go
make generate

# generate the CRD manifests at config/crd/bases/app.example.com_podsets.yaml
make manifest

# make changes to the controller
vim controllers/podset_controller.go

# run operator locally outside the cluster
make install run # first terminal
kubectl create -f config/samples/app_v1alpha1_podset.yaml # second terminal
kubectl get podset
kubectl get podset podset-sample -o yaml
kubectl get pods podset-sample-podqzwql -o yaml | grep ownerReferences -A5
kubectl patch podset podset-sample --type='json' -p '[{"op": "replace", "path": "/spec/replicas", "value": 5}]'
kubectl get pods
kubectl delete podsets.app.example.com podset-sample
make uninstall

# push operator image to registry
vim Makefile # update IMAGE_TAG_BASE
make docker-build docker-push

# run operator inside the cluster
make deploy
kubectl apply -f config/samples/app_v1alpha1_podset.yaml
make undeploy
```
