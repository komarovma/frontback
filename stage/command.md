
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


PVS-NFS

Start aplication
kubectl create -f postgres-configmap.yaml
#kubectl create -f postgres-storage-class.yaml
kubectl create -f postgres-storage-nfs.yaml
kubectl create -f postgres-statefulset.yaml
kubectl create -f postgres-service.yaml
kubectl create -f fb_dep.yaml
Delete aplication


kubectl exec postgres-statefulset-0 -- ls -la /data/pgdata
kubectl exec postgres-statefulset-0 -- sh -c 'echo "test2" > /data/pgdata/test2.txt'



clear
cat shared-volume.yaml
kubectl apply -f shared-volume.yaml
sleep 50
kubectl get po

kubectl exec test-shared-volumes -c nginx -- ls -la /ng/
kubectl exec test-shared-volumes -c busybox -- ls -la /bs/

kubectl exec test-shared-volumes -c nginx -- sh -c 'echo "test" > /ng/ng.txt'
kubectl exec test-shared-volumes -c busybox -- sh -c 'echo "test" > /bs/bs.txt'
kubectl exec test-shared-volumes -c nginx -- ls -la /ng/
kubectl exec test-shared-volumes -c busybox -- ls -la /bs/
kubectl delete -f shared-volume.yaml
