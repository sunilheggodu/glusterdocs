
GCS volume creation can be quickly done on a Kubernetes cluster by following the simple steps outlined below:


##**Step - 1: Create GCS cluster:**

-  Install the GCS deploy tool in one of the Kubernetes master node.

	    # sudo pip3 install kubectl-gluster

-  Create a GCS cluster configuration file with below information.

    -  namespace    - Cluster namespace, useful when managing multiple clusters.
    -  cluster-size - Number of nodes in Gluster cluster. Currently only 3 nodes are supported.
    -  nodes        - Details of nodes where Gluster server pods needs to be deployed.
    -  address      - Address of node as listed in kubectl get nodes(Use `kubectl get nodes` to get the address)
    -  devices      - Raw devices which are required to auto provision Gluster bricks during Volume create.

	Example:

		# File: mycluster.yaml
		---
		namespace: gcs
		cluster-size: 3
		nodes:
		    - address: kube1
		      devices: ["/dev/vdc"]

		    - address: kube2
		      devices: ["/dev/vdc"]

		    - address: kube3
		      devices: ["/dev/vdc"]


-  Create GCS cluster.

	    # kubectl gluster deploy <config-yaml>

-  Verify the GCS cluster.

	    # kubectl get pods -n gcs

	Note: All the pods should be in running state.


##**Step - 2: Create PVs from GCS cluster:**

-  Create a configuration file with appropriate information.

    Configuration file for file volume.

    Example:

		# File: pvc.yaml
		---
		kind: PersistentVolumeClaim
		apiVersion: v1
		metadata:
		  name: glusterfs-csi-pv
		spec:
		  storageClassName: glusterfs-csi
		  accessModes:
		  - ReadWriteMany
		  resources:
		    requests:
		    storage: 5Gi

-  Create persistent volume

		# kubectl create -f <config-yaml>

##**Step - 3: Verify the Volume:**

-  List the volume.

		# kubectl get pvc
