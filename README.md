#First App with Kubernets

```bash     
# criar imagem
docker build -t rafaelfabri/kub-first-app .
```

```bash     
-- criar um cluster local 
minikube start
```

```bash     
-- ver status 
minikube status

```bash     
-- criar um deployment de forma imperativa (isso nao vai funcionar) pq esta pegando pelo nome local da imagem 
kubectl create deployment first-app --image=kub-first-app
```

```bash     
quando fazemos esse comando a imagem kub-first-app nao vai ser pega do local host e enviada para a virtual machine -> na verdade com esse comando acima ele vai buscar no cluster por kub-first-app podemos ver os erros com o codigo abaixo
```
 
 ```bash     
-- com esse comando consigo olhar para os deploys
kubectl get deployments
kubectl get pods
```

para resolvermos isso podemos mandar a imagem para o docker hub
docker push rafaelfabri/kub-first-app
```
```bash     
kubectl create deployment first-app --image=rafaelfabri/kub-first-app
```



