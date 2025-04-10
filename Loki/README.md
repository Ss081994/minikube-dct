Lets setup Grafana and Loki!
To setup Grafana with loki, we will be using the grafana/loki-stack helm chart. In order to configure this chart run the below commands:

```sh
- helm repo add grafana https://grafana.github.io/helm-charts
- helm repo update

```

Once the above commands are executed, create a helm values file with the name loki-stack-values.yaml with the below content:
```sh
loki:
  enabled: true
  size: 1Gi
promtail:
  enabled: true
grafana:
  enabled: true
  sidecar:
    datasources:
      enabled: true
```

After the value file is created use the below helm command to deploy the loki-stack chart with the above mentioned configuration values.
```sh
helm upgrade --install loki --namespace=loki-stack grafana/loki-stack --values loki-stack-values.yaml --create-namespace 
```

Make sure to copy the complete command above and execute. Once done you should be able to see the following output for the command kubectl get pods -n loki-stack :
```sh 
NAME                            READY   STATUS    RESTARTS   AGE
loki-0                          1/1     Running   0          4m50s
loki-grafana-7b459df484-x2rc4   2/2     Running   0          4m50s
loki-promtail-ptl5m             1/1     Running   0          4m50s
```

Now lets also try to setup Grafana-Agent using the below helm value file. I named it as grafana-agent-values.yaml and its content are as below:

```sh
agent:
  # -- Mode to run Grafana Agent in. Can be "flow" or "static".
  mode: 'flow'
  configMap:
    # -- Create a new ConfigMap for the config file.
    create: true
    # -- Content to assign to the new ConfigMap.  This is passed into `tpl` allowing for templating from values.
    content: |
      
      logging {
        level  = "info"
        format = "logfmt"
      }

      loki.source.kubernetes_events "events" {
        log_format = "json"
        forward_to = [loki.write.loki_endpoint.receiver]
      }

      loki.write "loki_endpoint" {
        endpoint {
          url = "http://loki.loki-stack:3100/loki/api/v1/push"
        }
      }
```


In order to deploy the Grafana-Agent helm chart, use the below command with the above given values file.
helm repo add grafana https://grafana.github.io/helm-charts

```sh
helm repo update
helm upgrade --install grafana-agent --namespace=loki grafana/grafana-agent --values grafana-agent-values.yaml
```

To access the Grafana dashboard, execute the below command to forward the Grafana UI service from the minikube network to the localhost and then open localhost:3000 in your browser.

```sh
kubectl port-forward svc/loki-grafana 3000:80 -n loki-stack
```

You should be able to see the Grafana UI prompting for login. To login as admin, type admin for username and to get the admin password, execute the below command to extract the password from the secret created by the Grafana stack deployment.

```sh 
kubectl get secret loki-grafana -n loki-stack -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Once logged in, you gain access to the Grafana UI. You can then navigate to Explore tab and execute some LogQL after choosing Loki as the data source.
In order to view the events emitted by grafana-agent, use the below LogQL to fetch them all:
{job="loki.source.kubernetes_events"}
