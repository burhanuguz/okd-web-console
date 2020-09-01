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

![1](https://github.com/burhanuguz/okd-web-console/blob/master/pictures/1.png)
- Console will be available from NodePort 30000. Select either Host 1 or Host 2 like in the picture. Type 30000 and click Display Port.

![5](https://github.com/burhanuguz/okd-web-console/blob/master/pictures/5.png)
![6](https://github.com/burhanuguz/okd-web-console/blob/master/pictures/6.png)
- You will see OKD Web Console.

![2](https://github.com/burhanuguz/okd-web-console/blob/master/pictures/2.png)

![OLM](https://github.com/operator-framework/operator-lifecycle-manager/blob/master/logo.svg)

- You can view Operators also after you deploy OLM(OPERATOR LIFECYLE MANAGER) into the cluster from this link: [OLM Github Releases](https://github.com/operator-framework/operator-lifecycle-manager/releases/)

![4](https://github.com/burhanuguz/okd-web-console/blob/master/pictures/4.png)
