# First App with Kubernets

```bash     
# criar imagem
docker build -t rafaelfabri/kub-first-app .
```

```bash     
# criar um cluster local 
minikube start
```

```bash     
# ver status 
minikube status
```

```bash     
# criar um deployment de forma imperativa (isso nao vai funcionar) pq esta pegando pelo nome local da imagem 
kubectl create deployment first-app --image=kub-first-app
```


quando fazemos esse comando a imagem kub-first-app nao vai ser pega do local host e enviada para a virtual machine -> na verdade com esse comando acima ele vai buscar no cluster por kub-first-app podemos ver os erros com o codigo abaixo

 
 ```bash     
# com esse comando consigo olhar para os deploys
kubectl get deployments
kubectl get pods
```

para resolvermos isso podemos mandar a imagem para o docker hub

```bash     
docker push rafaelfabri/kub-first-app
```

```bash     
kubectl create deployment first-app --image=rafaelfabri/kub-first-app
```

comando para expor a porta do por meio de um service
```bash     
kubectl expose deployment first-app --type= --port=8080 --image=rafaelfabri/kub-first-app
```

Existem diferentes tipos de expose:
* --type=ClusterIP : somente exposto internamente dentro dodeployment
* --type=LoadBalancer : utiliza um LoadBalancer no cluster onde ele vai gerar um unico address para acessar o pod (so funciona se exister esse servico na infra -  AWS tem isso e minikube tbm) 

Para vermos as portas temos e links do LoadBalancer, temos que fazer o comando 

```bash     
kubectl get services
```

Porem, localmente vamos ter que executar o comando abaixo para o minikube entregar a url, isso [e um padrao do minikube, na cloud com um LoadBalancer real ja entregará o EXTERNAL-IP no comando acima 

```bash     
minikube service first-app
```




