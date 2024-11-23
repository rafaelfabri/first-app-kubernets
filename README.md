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

```bash     
minikube dashboard
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

Digitando esse comando acima foi entregue esse link http://192.168.49.2:30400/ existe um script no codigo se irmos para esse link http://192.168.49.2:30400/error conseguimos quebrar o codigo.

Isso pode causar um tempo de indisponibilidade do codigo, porem o minikube vai reiniciar o pod novamente em um tempo de alguns segundos.

{ Qual esta certo ? 
1 - Porem, a aplicacao ficara indisponivel por alguns segundos para manter ela podemos fazer multiplos container dentro com a mesma aplicacao no mesmo Pod, alem disso o LoadBalancer podera dividr o trafico entre os containers, para fazer isso o comando é:

2 - Porem, a aplicacao ficara indisponivel por alguns segundos para manter ela podemos fazer multiplos Pods assim quando um quebrar tera a mesma aplicacao em outro Pod, alem disso o LoadBalancer podera dividr o trafico entre Pods, para fazer isso o comando é
}

```bash     
kubectl scale deployment/first-app --replicas=3
```

Caso eu faca update do meu codigo localmente como posso fazer o update mandando pro cluster 

Temos que atualizar o imagem de dar o push novamente pro dockerhub
mas é FUNDAMENTAL adicionar uma tag nova pro kubernters identificar que houve um update na imagem e baixxar a mais recente 

```bash     
kubectl set image deployoment/first-app --kub-first-app=rafaelfabri/kub-first-app:tag
```

* --kub-first-app esse termo temos que ir la minikube dashboard e encontrar o nome da container que queremos atualizar  
