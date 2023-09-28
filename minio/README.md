
```diff
### with --set vars ###
# helm template minio bitnami/minio --namespace minio \
#  --set auth.rootUser=admin \
#  --set auth.rootPassword="Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS" \
#  --set service.type=LoadBalancer > minio-lb.yaml

### tar cahrt
helm pull bitnami/minio  --untar 

### edit vars in values.yaml ###
1)  type: LoadBalancer 
2) existingSecret: "minio-credentials"

sauera@sauera1MD6T minio-bitnami % cat values.yaml|grep minio-credentials
  existingSecret: "minio-credentials"
 
helm template minio bitnami/minio --namespace minio -f values.yaml > minio-lb.yaml

kubectl create -f namespace.yaml -n minio
kubectl create -f minio-secret.yaml -n minio
kubectl create -f minio-lb.yaml -n minio

kubectl get pods -n minio
NAME                     READY   STATUS    RESTARTS   AGE
minio-595c6845c4-rnvqc   1/1     Running   0          13h

kubectl get secret minio-credentials -n minio -o jsonpath="{.data.root-user}" | base64 --decode
admin%                                      

kubectl get secret minio-credentials -n minio -o jsonpath="{.data.root-password}" | base64 --decode
Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS%                                                                                                                                         
mc alias set myminio http://35.223.8.126:9000 admin Jd6WkDCkFHKDguZK4ZiGgSx1JyOLGSeWFCEnz7yS
Added `myminio` successfully.
 
## create bucket ###
kubectl create -f create-bucket-job.yaml  (svc = minio.minio.svc.cluster.local:9000.   the first "minio" is based on the helm release name)

validate 
sauera@sauera1MD6T minio-bitnami % mc ls myminio
[2023-09-06 08:05:06 CEST]     0B my-bucket/


```
