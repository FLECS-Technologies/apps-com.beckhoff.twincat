# Beckhoff Application

## Note: Edge Device Setup with Host Path

To ensure that licence / mosquitto confi of the host (Edge Device) is matched to the deployment, please currently do:

- 1:1 mapping of Deployment to Edge Host
- Spec Deployment with matching `nodeSelector`, and `volumes` config
- Node can be lable via e.g.: `kubectl label node NODE_NAME type=beckhoff-node-1`

Ref [`values.yaml`](./helm/values.yaml):
```yaml
    ### Ensure Node Alignment
    # label to matching node wiht initalized volumes (Currently only hostpath storage available)
    # how to label a node: kubectl label node NODE_NAME type=beckhoff-node-1
    nodeSelector:
      type: beckhoff-node-1
    ### Host Volume paths
    # host path at edge devices is mapped into /volumes-data
    volumes:
      licenseHostPath: /volumes-data/beckhoff/license
      # contains e.g.: mosquitto.conf
      mosquittoConfPath: /volumes-data/beckhoff/mosquitto

```
## Helpful commands

```bash
# Kubectl as container
docker run  --name  kubectl -v $(pwd):/mnt -t -d quay.io/kubermatic/util:2.5.1
docker exec -it kubectl bash

# Set Cluster kubeconfig
export KUBECONFIG=/mnt/kubeconfig-sa-beckhoff-sa-ap62qktwtj

# exec
kubectl exec -it -n beckhoff-twincat deployments/tc31-mtp -c tc31-mtp -- bash

# logs
## add -f for follow
kubectl logs -n beckhoff-twincat deployments/tc31-mtp tc31-mtp
kubectl logs -n beckhoff-twincat deployments/tc31-mtp mosquitto 

# portforward do local
kubectl -n beckhoff-twincat get pods
kubectl -n beckhoff-twincat port-forward deployments/tc31-mtp 1883:1883
```

