```diff

velero install \
  --provider aws \
  --plugins velero/velero-plugin-for-aws:master \
  --bucket velero \
  --secret-file ./credentials \
  --use-volume-snapshots=false \
  --dry-run -o yaml \
  --backup-location-config region=minio,s3ForcePathStyle="true",s3Url=http://minio.minio.svc.cluster.local:9000 > velero-install.yaml

kubectl create -f velero-crd.yaml
kubectl create -f velero-install.yaml -n velero

sauera@sauera1MD6T minio % cat backup-postgress.yaml 
apiVersion: velero.io/v1
kind: Backup
metadata:
  name: postgresql-pvc-backup
  namespace: velero
spec:
  includedNamespaces:
    - postgresql
  includedResources:
    - persistentvolumeclaims
  labelSelector:
    matchLabels:
      name: data-postgresql-0
  storageLocation: default
  ttl: 720h0m0s

sauera@sauera1MD6T minio % kubectl apply -f backup-postgress.yaml            
sauera@sauera1MD6T minio % 
sauera@sauera1MD6T minio % mc ls myminio/velero/backups/postgresql-pvc-backup
[2023-08-21 16:05:43 CEST]    29B STANDARD postgresql-pvc-backup-csi-volumesnapshotclasses.json.gz
[2023-08-21 16:05:43 CEST]    29B STANDARD postgresql-pvc-backup-csi-volumesnapshotcontents.json.gz
[2023-08-21 16:05:43 CEST]    29B STANDARD postgresql-pvc-backup-csi-volumesnapshots.json.gz
[2023-08-21 16:05:43 CEST] 2.7KiB STANDARD postgresql-pvc-backup-logs.gz
[2023-08-21 16:05:43 CEST]    29B STANDARD postgresql-pvc-backup-podvolumebackups.json.gz
[2023-08-21 16:05:43 CEST]    27B STANDARD postgresql-pvc-backup-resource-list.json.gz
[2023-08-21 16:05:43 CEST]    29B STANDARD postgresql-pvc-backup-volumesnapshots.json.gz
[2023-08-21 16:05:43 CEST]   116B STANDARD postgresql-pvc-backup.tar.gz
[2023-08-21 16:05:43 CEST] 3.3KiB STANDARD velero-backup.json
sauera@sauera1MD6T minio % 


```
