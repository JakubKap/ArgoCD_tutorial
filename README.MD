### ArgoCD Tutorial ###
Based on https://www.youtube.com/watch?v=MeU5_k9ssrs

### Purpose ###
ArgoCD is constantly checking changes residing on https://gitlab.com/nanuchi/argocd-app-config.git repository
and compare state of local with it. After that, if necessary it updates state of local repo.

### Assumptions ###
Project is created with an assumption that cluster will be automatically updated by 
ArgoCD only locally.

### Prerequisities ###
ArgoCD should be installed with following commands (after starting minikube locally):
```kubectl create namespace argocd```
```kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml```


### Accessing ArgoCD website ###
Fetch port of *argocd-server* with command ```kubectl get svc -n argocd```

C:\Users\kubix>kubectl get svc -n argocd
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.109.83.23     <none>        7000/TCP,8080/TCP            2m53s
argocd-dex-server                         ClusterIP   10.107.224.222   <none>        5556/TCP,5557/TCP,5558/TCP   2m53s
argocd-metrics                            ClusterIP   10.99.215.118    <none>        8082/TCP                     2m53s
argocd-notifications-controller-metrics   ClusterIP   10.106.153.54    <none>        9001/TCP                     2m53s
argocd-redis                              ClusterIP   10.97.211.82     <none>        6379/TCP                     2m53s
argocd-repo-server                        ClusterIP   10.105.246.250   <none>        8081/TCP,8084/TCP            2m53s
**argocd-server**                             ClusterIP   10.103.222.7     <none>        /TCP,<span style="color: red;">**443**</span>/TCP               2m53s
argocd-server-metrics                     ClusterIP   10.105.140.67    <none>        8083/TCP                     2m53s

and then port forward:
```kubectl port-forward -n argocd svc/argocd-server 8080:443```

Then, ArgoCD UI will be accessible through localhost:8080

Username is admin and password can be obtained by running following commands:
```kubectl get secret argocd-initial-admin-secret -n argocd -o yaml``` <= password encoded in Base64
```echo <PASSWORD_FROM_PREVIOUS_POINT> | base64 --decode```

### Applying ArgoCD configuration ###
```kubectl apply -f application.yaml```

### ArgoCD dashboard ###
![ArgoCD dashboard](./readme_assets/argo_dashboard.png)

//TODO Fix gitignore with AI
