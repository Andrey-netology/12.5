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


Inline-style: 
![alt text](https://github.com/Andrey-netology/12.5/blob/main/1.JPG "Logo Title Text 1")


Делаем проверку доступа:

Inline-style: 
![alt text](https://github.com/Andrey-netology/12.5/blob/main/2.JPG "Logo Title Text 1")

admin@kuber1:~$ sudo calicoctl get nodes
NAME    
kuber2  

admin@kuber1:~$ sudo calicoctl get ipPool
NAME           CIDR             SELECTOR   
default-pool   10.128.0.0/18   all()      

Inline-style: 
![alt text](https://github.com/Andrey-netology/12.5/blob/main/3.JPG "Logo Title Text 1")
