# edyoda-post-training-assessment
ASSINMENT 4-EXAMPLE VOTING APP

On kubectl apply -f .  the voting app pods-db, redis, result and vote- will be created.

deployment.apps/db created
service/db created
deployment.apps/redis created
service/redis created
deployment.apps/result created
service/result created
deployment.apps/vote created
service/vote created
deployment.apps/worker created

With kubectl get po we can get the active or running pods

[root@ip-172-31-22-24 k8s-specifications]# kubectl get po
NAME                      READY   STATUS    RESTARTS   AGE
db-b54cd94f4-49j4d        2/2     Running   0          15s
redis-868d64d78-6qs2s     2/2     Running   0          15s
result-5d57b59f4b-spzcw   2/2     Running   0          15s
vote-94849dc97-cbvn6      2/2     Running   0          15s
worker-dd46d7584-q8qjq    2/2     Running   0          15s


With kubectl get svc  we can observe that NodePort is assigned to voting and result pods or not. In My case Node port is assigned to result and vote app
[root@ip-172-31-22-24 k8s-specifications]# kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
db           ClusterIP   10.100.82.205    <none>        5432/TCP         9s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          38s
redis        ClusterIP   10.99.196.225    <none>        6379/TCP         9s
result       NodePort    10.109.127.125   <none>        5001:31001/TCP   9s
vote         NodePort    10.103.216.89    <none>        5000:31000/TCP   9s


Then after opening two separate browsers with the public IP with respective ports for Voting and result app we can vote and observe the votes.
 
  
  
  
  
  After deleting DB pod we will lose all the data in that db but the db pod will be restarted because of REPLICASET
  but now the result app will not work because it will be expecting the input from the db which has hot deleted and fpr that we have to delete the result app so that it restarts and connect tp new db pod.
  
  [root@ip-172-31-22-24 k8s-specifications]# kubectl delete po db-b54cd94f4-49j4d
pod "db-b54cd94f4-49j4d" deleted
 
  [root@ip-172-31-22-24 k8s-specifications]# kubectl get po
NAME                      READY   STATUS    RESTARTS   AGE
db-b54cd94f4-jhbpk        2/2     Running   0          22s
redis-868d64d78-6qs2s     2/2     Running   0          20m
result-5d57b59f4b-sl64k   2/2     Running   0          15m
vote-94849dc97-cbvn6      2/2     Running   0          20m
worker-dd46d7584-q8qjq    2/2     Running   1          20m
[root@ip-172-31-22-24 k8s-specifications]# kubectl delete po result-5d57b59f4b-sl64k
pod "result-5d57b59f4b-sl64k" deleted
^C
[root@ip-172-31-22-24 k8s-specifications]# kubectl get po
NAME                      READY   STATUS        RESTARTS   AGE
db-b54cd94f4-jhbpk        2/2     Running       0          2m38s
redis-868d64d78-6qs2s     2/2     Running       0          22m
result-5d57b59f4b-5k2nx   2/2     Running       0          8s
result-5d57b59f4b-sl64k   2/2     Terminating   0          18m
vote-94849dc97-cbvn6      2/2     Running       0          22m
worker-dd46d7584-k22kb    2/2     Running       0          118s
[root@ip-172-31-22-24 k8s-specifications]# ^C
[root@ip-172-31-22-24 k8s-specifications]#

  
  Result pod stopped after db deletion bcz it was expecting the input from the db pod (which was previously deleted)
 and to make the result app work we now have to delete the result app.
  
  
****
