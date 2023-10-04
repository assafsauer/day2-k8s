

```diff

#### Template HELM ####


 helm template loki grafana/loki -f  values.yaml   --namespace loki > loki-s3.yaml

#### Template ####
1) Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS (pass)
2) admin (user)
auth_enabled: false
....
 storage:
    bucketNames:
      chunks: chunks
      ruler: ruler
      admin: admin
    type: s3
    s3:
      s3: s3://admin:Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS@http://minio.minio.svc.cluster.local:9000/chunks
      endpoint: minio.minio.svc.cluster.local:9000
      region: null
      secretAccessKey: Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS
      accessKeyId: admin
      signatureVersion: null 
      s3ForcePathStyle: true
      insecure: true
      http_config: {}


#### additional Grafna CRDs requiered ####
git clone https://github.com/grafana/agent.git
kubectl create -f agent/production/operator/crds

#note run create or you will face this error:
#The CustomResourceDefinition "grafanaagents.monitoring.grafana.com" is invalid: metadata.annotations: Too long: must have at most 262144 bytes

kubectl create -f loki-s3.yaml -n loki


helm template promtail grafana/promtail \
    --set "loki.serviceName=loki" \
    --namespace=loki > promtail-manifests.yaml

kubectl apply -f promtail-manifests.yaml


OR:

## install promtail ##
  helm upgrade --install promtail grafana/promtail \
    --set "loki.serviceName=loki" \
    --namespace=loki
Release "promtail" does not exist. Installing it now.
NAME: promtail
LAST DEPLOYED: Tue Aug 15 14:11:29 2023
NAMESPACE: loki
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
***********************************************************************
 Welcome to Grafana Promtail
 Chart version: 6.14.1
 Promtail version: 2.8.3
***********************************************************************

Verify the application is working by running these commands:
* kubectl --namespace loki port-forward daemonset/promtail 3101
* curl http://127.0.0.1:3101/metrics

## verification ##

sauera@sauera1MD6T minio % kubectl get pods -n loki  
NAME                                          READY   STATUS      RESTARTS   AGE
grafana-agent-test-0                          2/2     Running     0          25m
grafana-agent-test-probe                      0/1     Completed   0          25m
loki-0                                        1/1     Running     0          4h29m
loki-1                                        1/1     Running     0          4h29m
loki-2                                        1/1     Running     0          4h29m
loki-backend-0                                1/1     Running     0          20m
loki-backend-1                                1/1     Running     0          20m
loki-backend-2                                1/1     Running     0          20m
loki-canary-29dfg                             1/1     Running     0          25m
loki-canary-cbq75                             1/1     Running     0          25m
loki-canary-mjr2v                             1/1     Running     0          25m
loki-gateway-78f986dd-dt6cx                   1/1     Running     0          25m
loki-grafana-agent-operator-d7c684bf9-4mpdq   1/1     Running     0          25m
loki-helm-test                                0/1     Error       0          25m
loki-logs-7dxl7                               2/2     Running     0          25m
loki-logs-nrrnv                               2/2     Running     0          25m
loki-logs-vxb65                               2/2     Running     0          25m
loki-read-659dcfb64b-bwdqk                    1/1     Running     0          25m
loki-read-659dcfb64b-h75zd                    1/1     Running     0          25m
loki-read-659dcfb64b-xqbhk                    1/1     Running     0          25m
loki-write-0                                  1/1     Running     0          25m
loki-write-1                                  1/1     Running     0          25m
loki-write-2                                  1/1     Running     0          25m
promtail-44tpf                                1/1     Running     0          4h17m
promtail-f5j8x                                1/1     Running     0          4h16m
promtail-n6pnt                                1/1     Running     0          4h16m

sauera@sauera1MD6T minio % kubectl get pvc -n loki                            
NAME                  STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-loki-backend-0   Bound    pvc-7c363dda-46c8-4ad3-9acc-c7041f446bc1   10Gi       RWO            standard-rwo   3m37s
data-loki-backend-1   Bound    pvc-44b73eba-b1eb-41f8-a087-538f19f4041c   10Gi       RWO            standard-rwo   3m37s
data-loki-backend-2   Bound    pvc-3de328f4-ddc8-496a-ade6-e51882c1a93b   10Gi       RWO            standard-rwo   3m36s
data-loki-write-0     Bound    pvc-4f136a7c-2d29-45d7-934e-a99fc4a56a51   10Gi       RWO            standard-rwo   3m37s
data-loki-write-1     Bound    pvc-02a7f78a-6781-4d29-a243-a38ab24fe9b1   10Gi       RWO            standard-rwo   3m37s
data-loki-write-2     Bound    pvc-57730c6c-3198-4b38-bb4e-5808250b1e2f   10Gi       RWO            standard-rwo   3m37s
sauera@sauera1MD6T minio % 


sauera@sauera1MD6T minio % mc ls myminio/chunks/                                
[2023-08-17 12:57:36 CEST]   251B STANDARD loki_cluster_seed.json
[2023-08-17 17:47:14 CEST]     0B fake/
[2023-08-17 17:47:14 CEST]     0B index/
[2023-08-17 17:47:14 CEST]     0B self-monitoring/
sauera@sauera1MD6T minio % 


```

<img width="589" alt="image" src="https://github.com/beeNotice/stacktic-templates/assets/22165556/77487a15-8754-446e-8bfc-9be4ebe73c58">

