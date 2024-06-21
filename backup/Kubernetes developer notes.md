Cheetsheet
https://kubernetes.io/docs/reference/kubectl/cheatsheet/ 
 
 kubectl get nodes -o wide

Login Azure cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
 
 
kubectl get nodes
kubectl get services  
kubectl apply -f azure-vote.yaml
kubectl get service azure-vote-front --watch
kubectl get secret
kubectl expose deployment eiot-backend --type=LoadBalancer --name=my-service
 https://kubernetes.io/docs/tutorials/stateless-application/expose-external-ip-address/
 
kubectl get secrets     
kubectl describe secret/db-credentials-c2hd54k784     
 
 
 
Apply file:
apiVersion:kustomize.config.k8s.io/v1beta1
kind:Kustomization
generatorOptions:
disableNameSuffixHash:true
labels:
foo:bar
secretGenerator:
-name:eiot-dev-credentials
literals:
-JWT_SECRET=123
kubectl apply -k .
 
 
access the pod’s bash shell:
kubectl exec -it [pod] -- /bin/bash
 
Get pod's logs:
kubectl logs -f  pinot-zookeeper-0 --namespace=pinot-quickstart 
 
 
Confimap:
kubectl get configmaps
kubectl describe configmaps eiot-dev-config
 
# Create the configmap from file
kubectl create configmap game-config --from-file=configure-pod-container/configmap/
Apply configmap yml:
 kubectl apply -k .
 
 
kubectl apply -f azure-vote.yaml
 
kubectl get pods --namespace=<insert-namespace-name-here>
 
Fixing terminating status
kubectl patch pv jhooq-pv -p '{"metadata":{"finalizers":null}}'
kubectl patch pvc jhooq-pv-claim -p '{"metadata":{"finalizers":null}}'
 
 
restart the  pod:
kubectl get pod pinot-controller-0 -n pinot-quickstart -o yaml | kubectl replace --force -f -
 
Create namespace
kubectl create ns pinot  
 
Add domain dns name:
(add it under annotations tag)con
 
service.beta.kubernetes.io/azure-dns-label-name: seveniotbackenddev
 
 
 
 
 
Latest eiot backend env values:
 
related kubectl  command:
 
seveniotbackendtest
 
kubectl apply -k .  --namespace=backend-app-node
kubectl logs -f <pod name> --namespace=backend-node
kubectl get pod <pod name> -n backend-node -o yaml | kubectl replace --force -f -
 
 
 
 
kubectl describe configmaps eiot-dev-config --namespace=backend-node
 
 
 
 
 
kubectl delete pod -n kube-system  -l k8s-app=kube-dns
 
 
Create tls lincese
 
openssl genrsa -out tls.key 2048
openssl req -new -key tls.key -out tls.csr
openssl x509 -req -days 365 -in tls.csr -signkey tls.key -out tls.crt
 
Install ingress
https://platform9.com/learn/v1.0/tutorials/nginix-controller-via-yaml
 
Caddy ingress
https://markheath.net/post/simple-aks-https-with-caddy
kubectl create namespace caddy-system
sudo kubectl apply -f mycaddy.yaml 
kubectl apply -f caddyingress_iot-api-dev.yaml
kubectl create secret  tls mycerts --key ./tls.key --cert ./tls.crt -n backend-app-node

Deleting all pods and ingress then add ingress with certs.

Fix error:
Error from server (InternalError): error when creating "caddyingress_iot-api-dev.yaml": Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": failed to call webhook: Post "[https://ingress-nginx-controller-admission.ingress-basic.svc:443/networking/v1/ingresses?timeout=10s](https://ingress-nginx-controller-admission.ingress-basic.svc/networking/v1/ingresses?timeout=10s)": service "ingress-nginx-controller-admission" not found
:
kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission


PFX file to crt file
https://ibm.com/docs/en/arl/9.7?topic=certification-extracting-certificate-keys-from-pfx-file


Spark setup
https://testdriven.io/blog/deploying-spark-on-kubernetes/

How to check space left of pod
kubectl -n pinot exec pinot-zookeeper-0 -- df -ah



Remove table from pinot with name like new
Change the logging level for docker thingstack
 
Get docker log file path - docker inspect --format='{{.LogPath}}' $INSTANCE_ID
Command to clear docker log - truncate -s 0 /home/document/path


How to run command inside the kubenetetes pod
 kubectl exec -it spark-master-59dd99bf6-j46p9 -- /bin/bash -c "ls"