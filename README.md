# tetris-game
some of step for the terris-game lab
step1: create a EKS cluster
step2: connect with EKS cluster with terminal
  aws eks update-kubeconfig --region region-code --name my-cluster

step3: install argoCD and use the documentation
    https://archive.eksworkshop.com/intermediate/290_argocd/install/
    #create namespace and install using manifast file
  kubectl create namespace argocd
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml

Step4: Expose the argoCD install DNS in the vaulue in variable ARGOCD_SRVER 
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    export ARGOCD_SERVER=`kubectl get svc argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`

step5: login into ArgoCD server 
  a) DNS
  using the DNS link stored in ARGOCD_SERVER
    echo $ARGOCD_SERVER
  
  b) get the password
    The initial password is autogenerated with the pod name of the ArgoCD API server:
      export ARGO_PWD=`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
  c) generate a username == admin
  argocd login $ARGOCD_SERVER --username admin --password $ARGO_PWD --insecure
    password: echo $ARGO_PWD





  
  

