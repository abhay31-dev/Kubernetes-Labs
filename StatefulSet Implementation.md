Lab 5: StatefulSet Implementation

Task 1: Creating a StatefulSet

wget files.cloudthat.training/devops/kubernetes-essentials/nginx-sts.yaml

kubectl apply -f nginx-sts.yaml

kubectl get service nginx-svc

kubectl get statefulset nginx-sts

kubectl get pods -w -l app=nginx-sts

for i in 0 1; do kubectl exec "nginx-sts-$i" -- sh -c 'hostname'; done

kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm

nslookup nginx-sts-0.nginx-svc

nslookup nginx-sts-1.nginx-svc

exit

kubectl delete pod -l app=nginx-sts

kubectl get pod -w -l app=nginx-sts

for i in 0 1; do kubectl exec "nginx-sts-$i" -- sh -c 'hostname'; done

Task 2: Writing to a Scalable Storage

kubectl get pvc -l app=nginx-sts

for i in 0 1; do kubectl exec "nginx-sts-$i" -- sh -c 'echo "$(hostname)" > /usr/share/nginx/html/index.html'; done

for i in 0 1; do kubectl exec -i -t "nginx-sts-$i" -- curl http://localhost/; done

Task 3: Scaling StatefulSet

kubectl scale sts nginx-sts --replicas=5

kubectl get pods -w -l app=nginx-sts

kubectl get pods -l app=nginx-sts

kubectl get pvc -l app=nginx-sts

kubectl edit sts nginx-sts

\# Set spec: replicas to 3, Press Esc and :wq! to exit vi

kubectl get pods -w -l app=nginx-sts

kubectl get pvc -l app=nginx-sts

Task 4: Rolling Update

kubectl edit sts nginx-sts

\# Change the spec: conatiners: image value to nginx:latest, Press Esc and :wq! to exit vi

kubectl get pod -l app=nginx-sts -w

kubectl get pods

kubectl describe pod nginx-sts-0 | grep Image

kubectl delete -f nginx-sts.yaml

Task 5: Clean-up Resources

kubectl delete -f nginx-sts.yaml

Lab 6: Namespaces and Resource Quotas in Kubernetes

Task 1: Creating a Namespace

kubectl create namespace quotas

kubectl get namespaces

Task 2: Creating a resourcequota

wget https://hpe-content.s3.ap-south-1.amazonaws.com/rq-quotas.yaml

Task 3: Verify resourcequota Functionality

wget https://hpe-content.s3.ap-south-1.amazonaws.com/rq-pod.yaml

Additional Topic: Installing Helm 3 in Ubuntu Linux and installing wordpress through Helm.

wget files.cloudthat.training/devops/kubernetes-essentials/helm.sh

cat helm.sh

bash helm.sh

helm version

helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update

helm repo list

helm search repo wordpress

helm install wordpress-chart bitnami/wordpress

helm uninstall wordpress-chart

####### IMPORTANT - DELETE THE CLUSTER FROM AWS #######

kops delete cluster \--yes

#if the command shows error, try

export KOPS\_STATE\_STORE=s3://< Your Cluster\_Name >

kops validate cluster

kops delete cluster \--yes

\# Verify all the AWS resources are deleted from AWS Console, in case of any confusions, feel free to reach us.

Source: https://raw.githubusercontent.com/NinjaCloud/k8s-prac/refs/heads/main/Kubernetes-Cheat-sheet.txt
