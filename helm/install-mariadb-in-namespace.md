# Install mariadb in namespace

## Walkthrough 

```
helm repo add bitnami https://charts.bitnami.com/bitnami
# jeder seinen eigenen namespace nimmt in der form
# z.B. db-<namenskuerzelodername>
helm upgrade --install my-mariadb bitnami/mariadb --version 16.5.0 --namespace=db-jochen --create-namespace 
```
