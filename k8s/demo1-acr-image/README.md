# Demo: Run the ACR Image

Deploy the same image students saw pass the Trivy gate in the container-security
session. The point: the same artifact that passed the pipeline is now running
inside AKS.

```
Code -> CI/CD -> Trivy Gate -> ACR -> AKS -> Pod
```

## Steps

1. Fill in the ACR image in [`deployment.yaml`](deployment.yaml):
   `<acr-name>.azurecr.io/<image-name>:<tag>`

2. If AKS needs pull access to that ACR and it isn't already wired up:

   ```bash
   az aks update -n <cluster-name> -g <resource-group> --attach-acr <acr-name>
   ```

3. Apply and inspect:

   ```bash
   kubectl apply -f deployment.yaml
   kubectl get pods
   kubectl describe pod <pod>
   ```

## Fallback — if the ACR image is gone

If the container-security ACR/image was deleted, rotated out, or is
unreachable, don't rebuild it live. In `deployment.yaml`, comment out the
ACR `image:` line and uncomment one of the public fallback images
(`mcr.microsoft.com/azuredocs/aci-helloworld` or `nginx:1.27-alpine`) —
both pull with no auth, no ACR attach needed. Re-run `kubectl apply`.

The lesson (pipeline artifact -> running pod) holds regardless of which
image lands in the pod. The YAML is not the point of the demo.
