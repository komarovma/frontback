
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
kubectl create -f fb_dep.yaml
Delete aplication

kubectl delete deploy frontback
kubectl delete svc postgres
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
