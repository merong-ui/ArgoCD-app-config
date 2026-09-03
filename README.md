# Commands used in this project:-

# install ArgoCD in k8s
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


# access ArgoCD UI
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# after argocd install, we use the last kubectl command to apply the application.yaml file in the claster.
kubectl apply -f application.yaml

# Argocd sucessfuly synced:-

<img width="1332" height="553" alt="Screenshot 2026-09-03 122218" src="https://github.com/user-attachments/assets/c33064d8-1f55-4c68-a0f5-087dc380c1ad" />

