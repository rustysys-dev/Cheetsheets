
# ISTIO

## Notes

### Mixer

Istio Mixer is responsible for enforcing various policy based control methods, and processing of telemetry data.  

Envoy Side-Cars cache policy data from Mixer that is used for each request, and buffer telemetry data to send to Mixer in bulk.

## Configuration
### *Enable log level setting on istio-proxy*
```bash
function log-level-change-k8s() {
    POD_NAME=$1
    NAMESPACE=$2
    LEVEL=$3
    L_TYPE=$4
    kubectl exec $(kubectl get pods -n $NAMESPACE \
                                    -l app=$POD_NAME \
                                    -o jsonpath='{.items[0].metadata.name}') \
                 -n $NAMESPACE -c istio-proxy \
                 -- curl -X POST localhost:15000/logging?$L_TYPE=$LEVEL -s
}
```

### *Add labels to a namespace*
```bash
kubectl label namespace solarmori istio-injection=enabled
```

### *Remove labels from a namespace*
```bash
kubectl label namespace solarmori istio-injection-
```

### *Disable automatic side-car injection for a pod*

* To disable automatic side-car injection for a specific pod, you should iinclude the following in its `spec`

```yaml
spec:
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
```

### *Whitelisting IP Addresses*

## Troubleshooting

### *Reasons for 503 Errors*

* Backend pod is not running successfully
* Connecting to resources outside of local namespace, it is required to use the full k8s DNS name.
    * i.e. ~~core-api-svc~~ -> core-api-svc.core-namespace.svc.cluster.local

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4Mzg2ODM0NDQsNjk4MDYyMjM0LC01Nj
k3Nzk1N119
-->