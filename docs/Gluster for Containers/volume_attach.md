
After [creating the volume](./volume_create.md) of the desired [type](./volume_types.md), user can use them in application pods by following the steps outlined below.


  - Create an application pod configuration file.

    Example:

		# File: app.yaml
		---
		apiVersion: v1
		kind: Pod
		metadata:
		  name: ta-redis
		  labels:
		    name: redis
		spec:
		  containers:
		    - name: redis
		      image: redis
		      imagePullPolicy: IfNotPresent
		      volumeMounts:
		        - mountPath: "/data"
		          name: glusterfscsivol
		  volumes:
		    - name: glusterfscsivol
		      persistentVolumeClaim:
		        claimName: glusterfs-csi-thin-pv

  -  Specify appropriate "claimName" in the configuration file along with other volume parameters.

	-  File Volumes  --  glusterfs-csi-pv
	-  Block Voumes  --  glusterblock-csi-pv
	-  Thin-Arbiter Volume  -- glusterfs-csi-thin-pv

  -  Create the application pod.

	    # kubectl create -f app.yaml

  -  Verify the application pod.

	    # kubectl get pods
