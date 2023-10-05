```diff

#kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.7/deploy/gatekeeper.yaml



## validate ##


kubectl get constrainttemplates.templates.gatekeeper.sh

NAME                AGE
k8srequiredlabels   35m
sauera@sauera1MD6T terraform %

 kubectl logs -l control-plane=controller-manager -n gatekeeper-system

{"level":"info","ts":1695980874.196917,"logger":"controller","msg":"handling constraint update","process":"constraint_controller","instance":{"apiVersion":"constraints.gatekeeper.sh/v1beta1","kind":"K8sRequiredLabels","name":"require-labels"}}
2023/09/29 09:48:51 http: TLS handshake error from 10.4.1.2:39350: EOF
{"level":"info","ts":1695980934.1812155,"logger":"controller","msg":"handling constraint update","process":"constraint_controller","instance": 

 kubectl get events -A  
prometheus     15s         Warning   FailedCreate             replicaset/prometheus-kube-state-metrics-547564f7d9                 Error creating: admission webhook "validation.gatekeeper.sh" denied the request: [require-labels] you must provide labels: {"app", "environment"}
prometheus     3m3s        Normal    ScalingReplicaSet        deployment/prometheus-kube-state-metrics                            Scaled up replica set prometheus-kube-state-metrics-547564f7d9 to 1
prometheus     17s         Warning   FailedCreate             daemonset/prometheus-node-exporter                                  Error creating: admission webhook "validation.gatekeeper.sh" denied the request: [require-labels] you must provide labels: {"app", "environment"}
spring         15s         Warning   FailedCreate             replicaset/spring-b4b678c8f                                         Error creating: admission webhook "validation.gatekeeper.sh" denied the request: [require-labels] you must provide labels: {"environment"}
spring         10s         Normal    EnsuringLoadBalancer     service/spring



```  
