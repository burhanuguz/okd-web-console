# OKD Web Console
Applying this YAML file will bring OKD Web Console(like openshift) to your cluster.

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: okd
  name: okd
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: okd
  replicas: 2
  template:
    metadata:
      labels:
        app: okd
    spec:
      containers:
      - image: quay.io/openshift/origin-console:latest
        name: okd
        readinessProbe:
          httpGet:
            path: /health
            port: 8443
            scheme: HTTP
          timeoutSeconds: 1
          periodSeconds: 10
          successThreshold: 1
          failureThreshold: 3
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 20m
            memory: 200Mi
        command:
          - /opt/bridge/bin/bridge
          - '--k8s-auth=service-account'
          - '--listen=http://0.0.0.0:8443'
          - '--public-dir=/opt/bridge/static'
        ports:
          - name: https
            containerPort: 443
            protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: okd
  namespace: kube-system
spec:
  selector:
    app: okd
  ports:
    - protocol: TCP
      nodePort: 30000
      port: 443
      targetPort: 8443
  type: NodePort
```
This can be check from [Katacoda Kubernetes Playground](https://www.katacoda.com/courses/kubernetes/playground)
- Deploy YAML to Kubernetes Cluster and wait for successful deployment.

![1](https://user-images.githubusercontent.com/59168275/91818196-2b7d9780-ec3e-11ea-8ef7-9189a0984c63.png)
- Console will be available from NodePort 30000. Select either Host 1 or Host 2 like in the picture. Type 30000 and click Display Port.

![5](https://user-images.githubusercontent.com/59168275/91818296-320c0f00-ec3e-11ea-887b-754d80cbe132.png)
![6](https://user-images.githubusercontent.com/59168275/91818172-291b3d80-ec3e-11ea-83b9-e727022706c8.png)
- You will see OKD Web Console.

![2](https://user-images.githubusercontent.com/59168275/91818221-2d475b00-ec3e-11ea-9686-30043f653c3a.png)

![OLM](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/logo.svg)

- You can view Operators also after you deploy OLM(OPERATOR LIFECYLE MANAGER) into the cluster from this link: [OLM Github Releases](https://github.com/operator-framework/operator-lifecycle-manager/releases/)

![4](https://user-images.githubusercontent.com/59168275/91818268-30424b80-ec3e-11ea-96dd-4dd275ed4758.png)
