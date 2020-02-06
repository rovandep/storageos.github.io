# OpenShift {{ page.oc-version }}

> Make sure the
> [prerequisites for StorageOS](/docs/prerequisites/overview) are
> satisfied before proceeding.

> If you have installed OpenShift {{ page.oc-version  }} in AWS ensure that the
> requisite ports are opened for the worker nodes' security group.


&nbsp;

Installing StorageOS on OpenShift {{ page.oc-version }} has fewer prerequisites
as compared to previous OpenShift 3.x versions.

The recommended way to run StorageOS on OpenShift {{ page.oc-version }} is to
deploy the StorageOS Cluster Operator using the OperatorHub and bootstrap
StorageOS using a Custom Resource.

Options:
- [OperatorHub (Recommended)](#operatorhub-install)
- [Manual install](#manual-install)


## OperatorHub install

StorageOS has a RedHat OpenShift certified operator in the OpenShift
OperatorHub. You can install the StorageOS operator through the OperatorHub.

1. Select the `OperatorHub` from the Catalog sub menu and search for StorageOS

    ![install-1](/images/openshift4/operatorhub.png)

1. Select StorageOS and click install

    ![install-2](/images/openshift4/install-storageos-operator.png)

1. Create the Operator subscription by clicking subscribe

    ![install-3](/images/openshift4/create-operator-subscription.png)

1. Wait until the Upgrade Status shows 1 installed

    ![install-4](/images/openshift4/install-complete.png)

1. Create a secret containing an `apiUsername` and an `apiPassword` key. The
   username and password defined in the secret will be used to authenticate
   when using the StorageOS CLI and GUI. Take note of which project you created
   the secret in.

    ![install-5](/images/openshift4/create-secret.png)
&nbsp;
    ![install-6](/images/openshift4/secret-inputs.png)

1. Go to `Installed Operators` and select the StorageOS operator. Select
   StorageOS Cluster and create a StorageOS cluster.

    ![install-7](/images/openshift4/create-stos.png)

1. The StorageOS cluster resource describes the StorageOS cluster that will be
   created. The `secretRefName` and `secretRefNamespace` should reference the secret
   containing the `apiUsername` and `apiPassword` that was previously created.

   Additional `spec` parameters are available on the [Cluster Operator
   configuration](/docs/reference/cluster-operator/configuration) page.

   ```bash
	apiVersion: storageos.com/v1
	kind: StorageOSCluster
	metadata:
	  name: storageos
	  namespace: openshift-operators
	spec:
	  secretRefName: "storageos-api" # Reference the Secret created in the previous step
	  secretRefNamespace: "openshift-operators"  # Namespace of the Secret created in the previous step
	  namespace: kube-system
	  csi:
	    enable: true
	    deploymentStrategy: deployment
	  resources:
	    requests:
	    memory: "512Mi"
	  k8sDistro: "openshift"
   ```

    ![install-8](/images/openshift4/stos-specs.png)


1. Verify that the StorageOS Resource enters a running state.

    ![install-9](/images/openshift4/stos-running.png)

1. {% include platforms/openshift4-selinux.md %}

&nbsp;
If this is your first installation you may wish to follow the [StorageOS
Volume guide](/docs/platforms/{{ page.platform }}/firstvolume/) for an example of how
to mount a StorageOS volume in a Pod.

&nbsp;

## Manual install

{% include operator/manual-install-disclaimer.md %}

{% include operator/install.md %}

