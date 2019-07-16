This is a Cluster Definition example. 

```bash
{{ page.cmd }} create -f - <<END
apiVersion: "storageos.com/v1"
kind: StorageOSCluster
metadata:
  name: "example-storageos"
  namespace: "storageos-operator"
spec:
  secretRefName: "storageos-api" # Reference from the Secret created in the previous step
  secretRefNamespace: "storageos-operator"  # Namespace of the Secret
  k8sDistro: "{{ page.platform }}"
  images:
    nodeContainer: "storageos/node:{{ site.latest_node_version }}" # StorageOS version
  sharedDir: '/var/lib/kubelet/plugins/kubernetes.io~storageos' # Needed when Kubelet as a container
  csi:
    enable: true
    deploymentStrategy: deployment
  resources:
    requests:
    memory: "512Mi"
#  nodeSelectorTerms:
#    - matchExpressions:
#      - key: "node-role.kubernetes.io/worker" # Compute node label will vary according to your installation
#        operator: In
#        values:
#        - "true"
END
```

> Additional `spec` parameters are available on the [Cluster Operator
> configuration]( {%link _docs/reference/cluster-operator/configuration.md %})
> page.

> You can find more examples such as deployments referencing a external etcd kv
> store for StorageOS in the [Cluster Operator examples](
> {%link _docs/reference/cluster-operator/examples.md %}) page.
