
GCS currently provides following types of volumes to cater container workload requirements.

-  [File Volumes](#file-volumes)
-  [Block Volumes](#block-volumes)
-  [Thin-Arbiter Volume](#ta-volumes)

<a name="file-volumes"></a>
##**File Volumes:**

  -  File volumes can be created by specifying the "storageClassName" as "glusterfs-csi" in the configuration file.

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

  -  Create the volume:

	    # kubectl create -f pvc.yaml

  -  Validate the File Volume creation:

	    # kubectl get pvc


<a name="block-volumes"></a>
##**Block Volumes:**

  -  Block volumes can be created by specifying the "storageClassName" as "glustervirtblock-csi" in the configuration file.

    Example:

		# File: pvc.yaml
		---
		kind: PersistentVolumeClaim
		apiVersion: v1
		metadata:
		  name: glusterblock-csi-pv
		spec:
		  storageClassName: glustervirtblock-csi
		  accessModes:
		  - ReadWriteMany
		  resources:
		    requests:
		      storage: 5Gi

  -  Create the volume:

	    # kubectl create -f pvc.yaml

  -  Validate the File Volume creation:

	    # kubectl get pvc


<a name="ta-volumes"></a>
##**Thin-Arbiter Volumes:**

Thin Arbiter volumes are new type of volumes where granularity is considered at brick level to decide the sanity of stored data.
It is essentially a type of File Volume.

  -  For Thin-Arbiter volumes storage class needs to be created with required parameters:

     Example:

		# File: thin-arbiter-storageclass.yaml
		---
		kind: StorageClass
		apiVersion: storage.k8s.io/v1
		metadata:
		  name: glusterfs-csi-thin-arbiter
		provisioner: org.gluster.glusterfs
		parameters:
		  arbiterType: "thin"
		  arbiterPath: "192.168.10.90:24007/mnt/arbiter-path"


  - It can be created by specifying the "storageClassName" as "glusterfs-csi-thin-arbiter" in the configuration file.

     Example:

		# File: thin-arbiter-pvc.yaml
		---
		kind: PersistentVolumeClaim
		apiVersion: v1
		metadata:
		  name: glusterfs-csi-thin-pv
		spec:
		  storageClassName: glusterfs-csi-thin-arbiter
		  accessModes:
		    - ReadWriteMany
		  resources:
		    requests:
		      storage: 5Gi


  -  Create the volume:

	    # kubectl create -f thin-arbiter-storageclass.yaml

  -  Validate the File Volume creation:

	    # kubectl get pvc
