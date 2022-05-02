Устанавливаем кластер Kuber:

admin@kuber1:~$ kubectl get nodes
NAME     STATUS   ROLES                  AGE    VERSION
kuber1   Ready    control-plane,master   112m   v1.23.6
kuber2   Ready    <none>                 17m    v1.23.6

Настраиваем политику как описано тут: https://docs.projectcalico.org/security/tutorials/kubernetes-policy-basic:

admin@kuber1:~$ kubectl create ns policy-hello
namespace/policy-hello created

Применяем приложение в нашу политику: 

admin@kuber1:~$ sudo kubectl apply --namespace=policy-hello -f ./ctrl/nginx_hello.yaml
deployment.apps/hello-world created


admin@kuber1:~$ sudo kubectl get pods --all-namespaces
NAMESPACE      NAME                             READY   STATUS    RESTARTS   AGE
kube-system    coredns-64897985d-pgkbc          1/1     Running   0          121m
kube-system    coredns-64897985d-tcr8g          1/1     Running   0          121m
kube-system    etcd-kuber1                      1/1     Running   0          121m
kube-system    kube-apiserver-kuber1            1/1     Running   0          121m
kube-system    kube-controller-manager-kuber1   1/1     Running   0          121m
kube-system    kube-flannel-ds-cm4mv            1/1     Running   0          117m
kube-system    kube-flannel-ds-prbkf            1/1     Running   0          26m
kube-system    kube-proxy-g5qps                 1/1     Running   0          26m
kube-system    kube-proxy-pzb4n                 1/1     Running   0          121m
kube-system    kube-scheduler-kuber1            1/1     Running   0          121m
policy-hello   hello-world-9456bbbf9-cp6xk      1/1     Running   0          16s
policy-hello   hello-world-9456bbbf9-fblfj      1/1     Running   0          16s
policy-hello   hello-world-9456bbbf9-xxjfr      1/1     Running   0          16s


Делаем проверку доступа:

admin@kuber1:~$ sudo kubectl run --namespace=policy-hello access --rm -ti --image busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget -q 10.128.0.19 -O -
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # 

admin@kuber1:~$ sudo calicoctl get nodes
NAME    
kuber2  

admin@kuber1:~$ sudo calicoctl get ipPool
NAME           CIDR             SELECTOR   
default-pool   10.128.0.0/18   all()      

admin@kuber1:~$ sudo calicoctl get profile
NAME                                                 
projectcalico-default-allow                          
kns.default                                          
kns.kube-node-lease                                  
kns.kube-public                                      
kns.kube-system                                      
kns.policy-my                                        
ksa.default.default                                  
ksa.kube-node-lease.default                          
ksa.kube-public.default                              
ksa.kube-system.attachdetach-controller              
ksa.kube-system.bootstrap-signer                     
ksa.kube-system.calico-kube-controllers              
ksa.kube-system.calico-node                          
ksa.kube-system.certificate-controller               
ksa.kube-system.clusterrole-aggregation-controller   
ksa.kube-system.coredns                              
ksa.kube-system.cronjob-controller                   
ksa.kube-system.daemon-set-controller                
ksa.kube-system.default                              
ksa.kube-system.deployment-controller                
ksa.kube-system.disruption-controller                
ksa.kube-system.dns-autoscaler                       
ksa.kube-system.endpoint-controller                  
ksa.kube-system.endpointslice-controller             
ksa.kube-system.endpointslicemirroring-controller    
ksa.kube-system.ephemeral-volume-controller          
ksa.kube-system.expand-controller                    
ksa.kube-system.generic-garbage-collector            
ksa.kube-system.horizontal-pod-autoscaler            
ksa.kube-system.job-controller                       
ksa.kube-system.kube-proxy                           
ksa.kube-system.namespace-controller                 
ksa.kube-system.node-controller                      
ksa.kube-system.nodelocaldns                         
ksa.kube-system.persistent-volume-binder             
ksa.kube-system.pod-garbage-collector                
ksa.kube-system.pv-protection-controller             
ksa.kube-system.pvc-protection-controller            
ksa.kube-system.replicaset-controller                
ksa.kube-system.replication-controller               
ksa.kube-system.resourcequota-controller             
ksa.kube-system.root-ca-cert-publisher               
ksa.kube-system.service-account-controller           
ksa.kube-system.service-controller                   
ksa.kube-system.statefulset-controller               
ksa.kube-system.token-cleaner                        
ksa.kube-system.ttl-after-finished-controller        
ksa.kube-system.ttl-controller                       
ksa.policy-my.default      
