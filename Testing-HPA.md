### Testing Horizontal Pod Scaler

Will create another POD with one container to do test, this time I'm not creating through deployment, Replication or daemonsets. but directly creating a single pod with one container

    apiVersion: v1
    kind: Pod
    metadata:
      name: demo
      namespace: my-kube
    spec:
      volumes:
      - name: shared-data
        emptyDir: {}
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
        - name: shared-data
          mountPath: /usr/share/nginx/html




