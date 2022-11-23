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

# scaffold CRD's API
kubebuilder create api --group=job --version=v1alpha1 --kind=At --controller --resource
```

## usage

- install prerequisites

- clone the repository

- make install

- make deploy

## resources

- [Writing a kubernetes controller in Go with kubebuilder](https://dev.to/ishankhare07/writing-a-simple-kubernetes-controller-in-go-with-kubebuilder-ib8)

- [What is a Kubernetes Operator?](https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/)

- [Demo Memcached Operator using Operator SDK](https://www.youtube.com/watch?v=9QR3sRp-6Xk&ab_channel=AustinMacdonald)

- [Build and deploy a basic operator](https://developer.ibm.com/learningpaths/kubernetes-operators/develop-deploy-simple-operator/create-operator/) and [Explanation of Memcached operator code](https://developer.ibm.com/learningpaths/kubernetes-operators/develop-deploy-simple-operator/deep-dive-memcached-operator-code/)
