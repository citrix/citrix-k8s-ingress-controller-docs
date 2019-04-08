# Logging

The logs generated by Citrix Ingress Controller(CIC) are available as part of [kubernetes logs](https://kubernetes.io/docs/concepts/cluster-administration/logging/). You can specify CIC to log in the following log levels:

    - CRITICAL
    - ERROR
    - WARNING
    - INFO
    - DEBUG

By default, CIC is set to log in `INFO` log level. If you want to specify CIC to log in a particular log level then you need to specify the log level in the CIC yaml file before deploying the CIC. You can specify the log level in the `spec` section of the yaml file as follows:

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: citrixingresscontroller
  labels:
    app: citrixingresscontroller
spec:
      serviceAccountName: cpx
      containers:
      - name: citrixingresscontroller
        image: "quay.io/citrix/citrix-k8s-ingress-controller:latest"
        env:
        # Set kube api-server URL
        - name: "kubernetes_url"
          value: "https://10.x.x.x:6443"
        # Set NetScaler Management IP
        - name: "NS_IP"
          value: "10.x.x.x"
        # Set log level
        - name: "LOGLEVEL"
          value: "DEBUG"
        - name: "EULA"
          value: "yes"
        args:
        - --feature-node-watch
          true
        imagePullPolicy: Always
```

## Modifying the log levels in CIC

To modify the log level configured on the CIC instance, you need to delete the instance and update the log level value in the following section and redeploy the CIC instance:

```YAML
# Set log level
- name: "LOGLEVEL"
  value: "XXXX"
```

Once you update the log level, save the YAML file and deploy it using the following command:

```
kubectl create -f citrix-k8s-ingress-controller.yaml
```