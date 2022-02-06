# NGINX Ingress with Keycloak k8s

Enable Oauth2 validation into k8s cluster with Nginx Ingress

## Enable Ingress Nginx  

```bash
Install Nginx-Ingress
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml

kubectl get pods -n ingress-nginx 
kubectl logs -n ingress-nginx -f ingress-nginx-controller-54d8b558d4-7w87n
```

## Deploy and delete pods frontend, keycloak and example service

```bash
kubectl delete -f frontend.yaml
kubectl delete -f keycloak.yaml
kubectl delete -f service-one.yaml

kubectl apply -f frontend.yaml
kubectl apply -f keycloak.yaml
kubectl apply -f service-one.yaml
```

## Generate CookieSecret  

Cookie secrete is used in oauth2-proxy.yaml with the following command  

```bash
python -c 'import os,base64; print(base64.b64encode(os.urandom(16)).decode("ascii"))'
```  

## Deploy and delete Ingress and OAuth2 proxy  

```bash
kubectl delete -f nginx_ingress_services.yaml
kubectl delete -f oauth2-ingress.yaml
kubectl delete -f oauth2-proxy.yaml

kubectl apply -f oauth2-proxy.yaml
kubectl apply -f oauth2-ingress.yaml
kubectl apply -f nginx_ingress_services.yaml
```  

## Getting token from Ketcloak  

```bash
POST: http://{ExposedIP}/auth/realms/master/protocol/openid-connect/token
x-www-form-urlencoded{
    client_id:{FromKeycloak}
    client_secret:{FromKeycloak}
    username:{User}
    password:{Password}
    grant_type:password
    scope:openid
}
```  

## Testing exposed service by postman or web browser  

```bash
http://{ExposedIP}/v1/service/microservicio/ping
```  

## Contributors  

Javier Rodrí­guez  
[hazelapd@gmail.com]  