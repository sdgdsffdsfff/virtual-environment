# Using virtual environment

**Premise**: Kubernetes cluster already has Istio installed, local machine has kubectl installed and configured, check below links for detail:

- Istio：https://istio.io/docs/setup/install
- kubectl：https://kubernetes.io/docs/tasks/tools/install-kubectl

## Add operator to cluster

Use `kubectl apply` command to add the operator into Kubernetes

```bash
kubectl apply -f https://virtual-environment.oss-cn-zhangjiakou.aliyuncs.com/release/0.1/env.alibaba.com_virtualenvironments_crd.yaml
kubectl apply -f https://virtual-environment.oss-cn-zhangjiakou.aliyuncs.com/release/0.1/operator.yaml
```

If the cluster has RBAC enabled, please also apply Role and ServiceAccount

```bash
kubectl apply -f https://virtual-environment.oss-cn-zhangjiakou.aliyuncs.com/release/0.1/service_account.yaml
kubectl apply -f https://virtual-environment.oss-cn-zhangjiakou.aliyuncs.com/release/0.1/role.yaml
kubectl apply -f https://virtual-environment.oss-cn-zhangjiakou.aliyuncs.com/release/0.1/role_binding.yaml
```

Now, the Kubernetes cluster already has capability to empower virtual environment.

## Create VirtualEnvironment instance

Create a resource with kind `VirtualEnvironment`, use `kubectl apply` command to take effect

```bash
kubectl apply -f path-to-virtual-environment-cr.yaml
```

After instance created, it would automatically watch all Service and Deployment resource **in the same Namespace** and generate isolation rule, thus form the virtual environment.

Please refer to [configuration guide](configuration.md) for the detail of the resource definition file, and modify the parameters according to the actual situation.

## Application adaptation

According to the virtual environment configuration, add a virtual environment label to the Pod and let the service pass the virtual environment header down through the call chain.

- Put a label in pod template of deployment to identify which virtual environment it belongs to (the default label key is `virtual-env`)

- Add mechanism for passing down environment name header in application (the default header key is `X-Virtual-Env`)

Check [quick start](quickstart.md) for a completed demonstrate.
