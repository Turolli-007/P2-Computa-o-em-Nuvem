# P2-Computa-o-em-Nuvem

# Questoes escritas 

1 - A diferenca entre a IaaS e a SaaS e que a IaaS e utilizado em infraestrutura como Servidores e Maquinas virtuais ja a SaaS ela utiliza software prontos da intenet como por exemplo Gmail, Google Docs e Microsoft 365

2- Nuvem Hibrida

3- Máquina Virtual possui um sistema operacional completo para cada máquina virtual ja o Conteiner compartilha o sistema operacional do host, sendo mais leve e iniciando mais rápido

6 - 1 - A imagem Docker é o modelo usado para criar contêineres, enquanto o contêiner é a execução desse modelo em um ambiente isolado

2 - Um Deployment gerencia os Pods, garantindo a quantidade desejada de réplicas, realizando atualizações e permitindo a recuperação automática caso algum Pod falhe

3 - Quando exclui um Pod ele se recria novamente porem com nome diferente e isso aconteceu por que ele estava sendo gerenciado e monitorado pelo Deployment

4 - A finalidade de um Service no Kubernetes é permitir a comunicação e o acesso aos Pods fornecendo uma forma estável de conexão mesmo quando os Pods são recriados ou possuem novos endereços IP

5 - Conclusão 

aprendi a criar um conteiner temporario, a como acessar ele e a como personalisar. Ja com o kubernets aprendi a como criar um deployment e a como verificar os pods que o deployment gerencia, tambem foi ensinado a como criar e deletar replicas de um pod 
e assm aprendendo como funciona a autorecuperção desse pod, foi ensinado a criar um service com acesso a http.

# Questões Praticas

Docker - 

Evidência 1 - 

root@controlplane:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
a55d6706e158   nginx:alpine   "/docker-entrypoint.…"   3 seconds ago   Up 3 seconds   0.0.0.0:8090->80/tcp, [::]:8090->80/tcp   comunicado-web

Evidência 2

root@controlplane:~$ curl http://localhost:8090
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

Evidência 3

root@controlplane:~$ kubectl get nodes
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   13d   v1.37.1
node01         Ready    <none>          13d   v1.37.1

Evidência 4 

kubectl scale deployment comunicado-web --replicas=3
deployment.apps/comunicado-web scaled

kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-259hw   1/1     Running   0          21s
comunicado-web-57c76f7bc4-nqw42   1/1     Running   0          21s
comunicado-web-57c76f7bc4-xvqk2   1/1     Running   0          87s

Evidência 5 

root@controlplane:~$ kubectl delete pod comunicado-web-57c76f7bc4-259hw
pod "comunicado-web-57c76f7bc4-259hw" deleted from default namespace
root@controlplane:~$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-nqw42   1/1     Running   0          3m22s
comunicado-web-57c76f7bc4-xllvs   1/1     Running   0          11s
comunicado-web-57c76f7bc4-xvqk2   1/1     Running   0          4m28s

Evidência 6 

root@controlplane:~$ kubectl get services 
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   13d


root@controlplane:~$ kubectl run teste --image=curlimages/curl --rm -it --restart=Never -- curl http://comunicado-service 
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
warning: couldn't attach to pod/teste, falling back to streaming logs: unable to upgrade connection: container teste not found in pod teste_default
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
pod "teste" deleted from default namespace
