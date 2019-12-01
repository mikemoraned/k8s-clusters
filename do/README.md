# Digital Ocean Cluster

## Create Cluster

    CLUSTER_NAME=cluster-20191201
    REGION_NAME=lon1
    doctl kubernetes cluster create ${CLUSTER_NAME} --region ${REGION_NAME}
    kubectl config use-context do-${REGION_NAME}-${CLUSTER_NAME}

## Install helm locally

    brew install helm
    helm repo add stable https://kubernetes-charts.storage.googleapis.com/
    helm repo update

## Setup ingress

    kubectl apply -f ./ingress-nginx/mandatory.yaml
    kubectl apply -f ./ingress-nginx/cloud-generic.yaml
    kubectl get svc --namespace=ingress-nginx

## Setup cert-manager with prod and staging issuers

    kubectl apply -f certmanager/00-crds.yaml
    kubectl create namespace cert-manager

    kubectl get pods --namespace cert-manager

    helm repo add jetstack https://charts.jetstack.io
    helm repo update
    helm install jetstack/cert-manager --generate-name -n cert-manager

Test setup

    kubectl apply -f certmanager/test-resources.yaml
    kubectl describe certificate -n cert-manager-test
    kubectl delete -f certmanager/test-resources.yaml

    kubectl create -f certmanager/letsencrypt-staging-issuer.yaml
    kubectl describe clusterissuer letsencrypt-staging

    kubectl create -f certmanager/letsencrypt-prod-issuer.yaml
    kubectl describe clusterissuer letsencrypt-prod

## Delete cluster

    doctl kubernetes cluster delete ${CLUSTER_NAME}

Also need to:

- Manually delete created load-balancer
