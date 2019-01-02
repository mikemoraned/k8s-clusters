# Digital Ocean

    doctl help kubernetes cluster create
    doctl kubernetes cluster create do-k8s-play-ingress --tag tutorial
    kubectl config use-context do-nyc1-do-k8s-play-ingress
    kubectl get nodes
    kubectl create -f echo1.yaml
    kubectl get svc echo1
    kubectl create -f echo2.yaml
    kubectl get svc
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/provider/cloud-generic.yaml
    kubectl get svc --namespace=ingress-nginx
    kubectl apply -f echo_ingress.yaml
    curl echo1.do.houseofmoran.io
    curl echo2.do.houseofmoran.io

    kubectl -n kube-system create serviceaccount tiller
    kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    helm init --service-account tiller
    kubectl get pods --namespace kube-system
    helm install --name cert-manager --namespace kube-system stable/cert-manager
    kubectl create -f prod_issuer.yaml
    kubectl apply -f echo_ingress.yaml
    kubectl describe certificate letsencrypt-prod
