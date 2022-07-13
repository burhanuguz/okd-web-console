# OKD Web Console
Change NODE-IP in the ingress definition to your worker node IP. After that, deploying okd.yaml file will bring OKD Web Console(like openshift) to your cluster.

```bash
kubectl create serviceaccount -n kube-system okd-admin
kubectl create clusterrolebinding okd-admin --clusterrole=cluster-admin --serviceaccount=kube-system:okd-admin
kubectl create -f https://raw.githubusercontent.com/burhanuguz/okd-web-console/master/okd.yaml
kubectl expose deployment -n kube-system okd --name=okd --port=443 --target-port=8443
kubectl create ingress okd-admin --class=nginx --rule="console-okd.local.gd/*=okd:443" --rule="console-okd.NODE-IP.nip.io/*=okd:443" -n kube-system
```

As an example you can check console on [Katacoda Kubernetes Playground](https://www.katacoda.com/courses/kubernetes/playground)
- Deploy YAML to Kubernetes Cluster and wait for successful deployment.
- Console will be available from [http://console-okd.NODE-IP.nip.io](http://console-okd.NODE-IP.nip.io).

![2](https://user-images.githubusercontent.com/59168275/91818221-2d475b00-ec3e-11ea-9686-30043f653c3a.png)

- You can view Operators also after you deploy OLM(OPERATOR LIFECYLE MANAGER) into the cluster from this link: [OLM Github Releases](https://github.com/operator-framework/operator-lifecycle-manager/releases/)

![4](https://user-images.githubusercontent.com/59168275/91818268-30424b80-ec3e-11ea-96dd-4dd275ed4758.png)
