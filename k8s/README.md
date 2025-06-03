# Beckhoff Application

## Note: Edge Device Setup with Host Path

To ensure that licence / mosquitto confi of the host (Edge Device) is matched to the deployment, please currently do:

- 1:1 mapping of Deployment to Edge Host
- Spec Deployment with matching `nodeSelector`, and `volumes` config
- Node can be lable via e.g.: `kubectl label node NODE_NAME type=beckhoff-node-1`

Ref [`values.yaml`](./helm/values.yaml):
```
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