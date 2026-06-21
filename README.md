# orders-service-deploy

Repositorio de despliegue GitOps con Helm y ArgoCD para `orders-service`.

## Estructura

- `charts/orders-service`: chart Helm del microservicio
- `argocd/application.yaml`: aplicacion de ArgoCD para sincronizacion automatica

## Helm

```bash
helm install orders-dev ./charts/orders-service -f ./charts/orders-service/values-dev.yaml
helm install orders-prod ./charts/orders-service -f ./charts/orders-service/values-prod.yaml --set image.tag=1.0.0
helm upgrade orders-prod ./charts/orders-service -f ./charts/orders-service/values-prod.yaml --set image.tag=1.1.0
helm rollback orders-prod 1
```

## ArgoCD

Instalacion rapida:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f argocd/application.yaml
```

## Flujo GitOps

El pipeline del repo de aplicacion modifica `values-prod.yaml` con el nuevo tag de imagen.
ArgoCD detecta ese commit y aplica el cambio en Kubernetes automaticamente.
