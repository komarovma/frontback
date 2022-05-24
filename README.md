<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="____131___deployment_statefulset_services_endpoints_0"></a>Домашнее задание к занятию “13.1 контейнеры, поды, deployment, statefulset, services, endpoints”</h2>
<p class="has-line-data" data-line-start="2" data-line-end="3">Клонируем</p>
<pre><code>https://github.com/netology-code/devkub-homeworks/tree/main/13-kubernetes-config
</code></pre>
<p class="has-line-data" data-line-start="6" data-line-end="7">Собираем в Docker Image</p>
<pre><code>docker-compose up --build
</code></pre>
<p class="has-line-data" data-line-start="9" data-line-end="10">Смотрим Image</p>
<pre><code>docker image ls
13-kubernetes-config_frontend        latest                                                  ca831218c556   2 minutes ago   142MB
13-kubernetes-config_backend         latest                                                  fb6345acb66f   3 minutes ago   1.08GB
</code></pre>
<p class="has-line-data" data-line-start="14" data-line-end="15">Входим в докер Hub</p>
<pre><code>docker login
</code></pre>
<p class="has-line-data" data-line-start="17" data-line-end="18">Тэгируем для репозитория</p>
<pre><code>docker tag ca831218c556 komarovma/devops-netology:frontend 
docker tag fb6345acb66f komarovma/devops-netology:backend 
</code></pre>
<p class="has-line-data" data-line-start="21" data-line-end="22">Пушим в репозиторий</p>
<pre><code>docker image push komarovma/devops-netology:frontend
docker image push komarovma/devops-netology:backend 
</code></pre>
<p class="has-line-data" data-line-start="25" data-line-end="26">Строка из репозитория</p>
<pre><code>docker pull komarovma/devops-netology:frontend
docker pull komarovma/devops-netology:backend
</code></pre>
<p class="has-line-data" data-line-start="30" data-line-end="34">Большое количество файлов обусловлено учебной задачей, с целью более глубокого понимания.<br>
В директории stage набор YAML файлов для создание приложения<br>
Первый под содержит в себе 2 контейнера — фронтенд, бекенд;<br>
Второй под база данных — через statefulset и службы.</p>
<p class="has-line-data" data-line-start="35" data-line-end="36">Start aplication</p>
<pre><code>kubectl create -f postgres-configmap.yaml
kubectl create -f postgres-storage.yaml
kubectl create -f postgres-statefulset.yaml
kubectl create -f postgres-service.yaml
kubectl create -f fb_dep.yaml
</code></pre>
<p class="has-line-data" data-line-start="43" data-line-end="44">Просмотр состояния</p>
<pre><code>kubectl get deploy
kubectl get svc
kubectl get statefulset
kubectl get PersistentVolumeClaim
kubectl get PersistentVolume 
kubectl get configmap
</code></pre>
<p class="has-line-data" data-line-start="52" data-line-end="53">Delete aplication</p>
<pre><code>kubectl delete deploy frontback
kubectl delete svc postgres
kubectl delete statefulset postgres-statefulset
kubectl delete PersistentVolumeClaim postgres-pv-claim
kubectl delete PersistentVolume postgres-pv-volume
kubectl delete configmap postgres-config
</code></pre>
<p class="has-line-data" data-line-start="61" data-line-end="65">В директории prod набор YAML файолов для создание приложения каждый компонент (база, бекенд, фронтенд) запускаются в своем поде<br>
Для связи используются service (у каждого компонента свой);<br>
В окружении фронта прописан адрес сервиса бекенда;<br>
В окружении бекенда прописан адрес сервиса базы данных.</p>
<p class="has-line-data" data-line-start="66" data-line-end="67">Start aplication</p>
<pre><code>kubectl create -f postgres-configmap.yaml
kubectl create -f postgres-storage.yaml
kubectl create -f postgres-statefulset.yaml
kubectl create -f postgres-service.yaml
kubectl create -f back.yaml
kubectl create -f front.yaml
</code></pre>
<p class="has-line-data" data-line-start="75" data-line-end="76">Просмотр состояния</p>
<pre><code>kubectl get deploy
kubectl get svc
kubectl get statefulset
kubectl get PersistentVolumeClaim
kubectl get PersistentVolume 
kubectl get configmap
</code></pre>
<p class="has-line-data" data-line-start="84" data-line-end="85">Delete aplication</p>
<pre><code>kubectl delete deploy front
kubectl delete deploy back
kubectl delete svc postgres
kubectl delete svc front
kubectl delete svc back
kubectl delete statefulset postgres-statefulset
kubectl delete PersistentVolumeClaim postgres-pv-claim
kubectl delete PersistentVolume postgres-pv-volume
kubectl delete configmap postgres-config
</code></pre>
<p class="has-line-data" data-line-start="96" data-line-end="97">Результат</p>
<pre><code>mike@HOMEDX79SR:~/frontback/prod$ kubectl get pods -o=wide
NAME                     READY   STATUS    RESTARTS   AGE     IP            NODE    NOMINATED NODE   READINESS GATES
back-5f9757fcdb-gxqqv    1/1     Running   0          8m18s   10.233.90.2   node1   &lt;none&gt;           &lt;none&gt;
back-5f9757fcdb-vhbt2    1/1     Running   0          8m18s   10.233.96.3   node2   &lt;none&gt;           &lt;none&gt;
front-7c8b84bbf8-tgthj   1/1     Running   0          8m9s    10.233.92.5   node3   &lt;none&gt;           &lt;none&gt;
postgres-statefulset-0   1/1     Running   0          8m40s   10.233.92.4   node3   &lt;none&gt;           &lt;none&gt;

mike@HOMEDX79SR:~/frontback/prod$ kubectl get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
back    2/2     2            2           5m10s
front   1/1     1            1           5m1s
mike@HOMEDX79SR:~/frontback/prod$ kubectl get svc
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
back         NodePort       10.233.60.210   &lt;none&gt;        8080:31593/TCP   5m17s
front        NodePort       10.233.58.126   &lt;none&gt;        8080:31076/TCP   5m8s
kubernetes   ClusterIP      10.233.0.1      &lt;none&gt;        443/TCP          7h40m
postgres     LoadBalancer   10.233.28.103   &lt;pending&gt;     5432:32291/TCP   5m37s
mike@HOMEDX79SR:~/frontback/prod$ kubectl get statefulset
NAME                   READY   AGE
postgres-statefulset   1/1     5m45s
mike@HOMEDX79SR:~/frontback/prod$ kubectl get PersistentVolumeClaim
NAME                STATUS   VOLUME               CAPACITY   ACCESS MODES   STORAGECLASS   AGE
postgres-pv-claim   Bound    postgres-pv-volume   5Gi        RWX            manual         5m50s
mike@HOMEDX79SR:~/frontback/prod$ kubectl get PersistentVolume
NAME                 CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                       STORAGECLASS   REASON   AGE
postgres-pv-volume   5Gi        RWX            Retain           Bound    default/postgres-pv-claim   manual                  5m55s
mike@HOMEDX79SR:~/frontback/prod$ kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      7h40m
postgres-config    3      6m3s
mike@HOMEDX79SR:~/frontback/prod$</code></pre>