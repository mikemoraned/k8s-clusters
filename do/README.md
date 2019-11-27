# Digital Ocean Cluster

## Create Cluster

    CLUSTER_NAME=cluster-with-secure-ingress-and-monitoring
    REGION_NAME=lon1
    doctl kubernetes cluster create ${CLUSTER_NAME} --region ${REGION_NAME}
    kubectl config use-context do-${REGION_NAME}-${CLUSTER_NAME}

## Install helm / tiller

    kubectl -n kube-system create serviceaccount tiller
    kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    helm init --service-account tiller
    kubectl get pods --namespace kube-system

## Setup monitoring

(Based on [Kubernetes Tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-digitalocean-kubernetes-cluster-monitoring-with-helm-and-prometheus-operator))

    helm install --namespace monitoring --name doks-cluster-monitoring -f monitoring/custom-values.yaml stable/prometheus-operator

### Access monitoring locally

    kubectl port-forward -n monitoring svc/doks-cluster-monitoring-pr-prometheus 9090:9090 &
    kubectl port-forward -n monitoring svc/doks-cluster-monitoring-grafana 9091:80 &
    kubectl port-forward -n monitoring svc/doks-cluster-monitoring-pr-alertmanager 9093:9093 &

## Setup ingress

    kubectl apply -f ./ingress-nginx/mandatory.yaml
    kubectl apply -f ./ingress-nginx/cloud-generic.yaml
    kubectl get svc --namespace=ingress-nginx

## Setup cert-manager with prod and staging issuers

    helm install --name cert-manager --namespace kube-system stable/cert-manager
    kubectl create -f certmanager/letsencrypt-staging-issuer.yaml
    kubectl create -f certmanager/letsencrypt-prod-issuer.yaml

## Delete cluster

    doctl kubernetes cluster delete ${CLUSTER_NAME}

Also need to:

- Manually delete created load-balancer
