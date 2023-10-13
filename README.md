DevOps Project - Проект по предметот Континуирана интеграција и испорака. 

Опис на проектот: Апликацијата претставува едноставна book review апликација како онлајн платформа каде што корисниците може да ставаат review на прочитана книга. 

React апликација со Node.js како backend, заедно со апликацијата има и Prometheus за мониторирање, а употребени се Helm charts и Kubernetes (minikube) за deployment. 

Docker

Серверот е докеризиран во соодветен docker image преку следниот Dockerfile. 
Клиентот е докеризиран во соодветен docker image преку следниот Dockerfile.

Kubernetes (minikube)

Апликацијата се deploy-нува на локален minikube кластер кој што се инсталира со следните команди.
Серверот, клиентот и Prometheus се deploy-нати на minikube кластерот. Клиентот ја користи портата
3000, серверот ја користи портата 3001, а Prometheus портата 9090. 

Prometheus

Со следните наредби е конфигуриран Prometheus и ја користи портата 9090.
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install prometheus bitnami/kube-prometheus
kubectl port-forward --namespace default svc/prometheus-kube-prometheus-prometheus 9090:9090

Github actions

За овој дел креирав GitHub action pipeline кој на секој push на main branch прво инсталира minikube,
после тоа креира нов docker image и го deploy-нува на minikube за серверот и истото и за клиентот,
а потоа го deploy-нува Prometheus на minikube директно на github runner-от.
Клиентот ја користи портата 3000, серверот ја користи портата 3001, а Prometheus портата 9090.

Helm

За deployment на апликацијата на minikube користев Helm charts.
Helm chart-овите се креирани со helm create deploy наредбата.
Во values.yaml е внесено името на docker image-ot кој е избилдан од претходно, како и портата на
која што слуша апликацијата.
Helm charts има и за клиентот и за серверот бидејќи посебно се deploy-нуваат. 
