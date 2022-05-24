
Delete
kubectl get deploy
kubectl delete deploy frontback

kubectl get svc
kubectl delete svc postgres

kubectl get statefulset
kubectl delete statefulset postgres-statefulset

kubectl get PersistentVolumeClaim
kubectl delete PersistentVolumeClaim postgres-pv-claim

kubectl get PersistentVolume 
kubectl delete PersistentVolume postgres-pv-volume

kubectl get configmap
kubectl delete configmap postgres-config


Start aplication
kubectl create -f postgres-configmap.yaml
kubectl create -f postgres-storage.yaml
kubectl create -f postgres-statefulset.yaml
kubectl create -f postgres-service.yaml
kubectl create -f back.yaml
kubectl create -f front.yaml

Delete aplication

kubectl delete deploy front
kubectl delete deploy back
kubectl delete svc postgres
kubectl delete svc front
kubectl delete svc back
kubectl delete statefulset postgres-statefulset
kubectl delete PersistentVolumeClaim postgres-pv-claim
kubectl delete PersistentVolume postgres-pv-volume
kubectl delete configmap postgres-config

Просмотр состояния
kubectl get deploy
kubectl get svc
kubectl get statefulset
kubectl get PersistentVolumeClaim
kubectl get PersistentVolume 
kubectl get configmap
