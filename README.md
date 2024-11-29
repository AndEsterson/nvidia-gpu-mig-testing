# nvidia-gpu-mig-testing

Sets up mig on a k3s cluster (using forked images by default), lots of this work heavily used [a CERN bblog](https://kubernetes.web.cern.ch/blog/2023/03/17/efficient-access-to-shared-gpu-resources-part-2/)

# Usage

```
helm install -f ~/gpu-operator-mig-values.yaml --wait --generate-name \
    -n gpu-operator --create-namespace \
    nvidia/gpu-operator
```

```
kubectl apply -f ./configs/nvidia-mig-config.yaml
```

now label nodes to apply mig presets, e.g

```
kubectl label node <NAME> nvidia.com/mig.config=1g.10gb
```

If working, the node should have mig gpus listed under `Allocatable` (note pods will have to allocate mig gpus, rather than just `nvidia.com/gpu`, using `strategy: single` will use MIG across all GPUs and allow for scheduling as `nvidia.com/gpu`)
