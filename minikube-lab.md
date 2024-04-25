Claro! Abaixo está um script Python para realizar o laboratório descrito no tutorial utilizando um Colab Notebook. Certifique-se de ter o `kubectl` configurado corretamente, pois o Colab Notebook executa em um ambiente baseado em nuvem e pode ser necessário instalar o `minikube` e o `kubectl` se não estiverem disponíveis.

```python
# Instalando o minikube e o kubectl
!curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && sudo install minikube-linux-amd64 /usr/local/bin/minikube
!curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
  && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

# Iniciando o minikube
!minikube start

# Criando um Deployment
!kubectl create deployment hello-node --image=registry.k8s.io/echoserver:1.4

# Expondo o Deployment como um Serviço
!kubectl expose deployment hello-node --type=LoadBalancer --port=8080

# Obtendo o endereço IP externo do serviço
external_ip = None
while external_ip is None:
    output = !kubectl get svc hello-node
    if len(output) > 0:
        fields = output[1].split()
        external_ip = fields[3]
        if external_ip != "<pending>":
            break
    print("Aguardando endereço IP externo...")
    time.sleep(10)

# Exibindo o endereço IP externo e a porta do serviço
print("Endereço IP externo:", external_ip)
print("Porta do serviço: 8080")

# Abrindo o serviço no navegador (apenas para ambiente local)
!minikube service hello-node

# Parando o minikube (opcional)
#!minikube stop

# Apagando a máquina virtual do minikube (opcional)
#!minikube delete
```

Este script Python irá instalar o `minikube` e o `kubectl` se ainda não estiverem instalados, iniciar o Minikube, criar um Deployment com a imagem do echoserver, expor o Deployment como um Serviço do tipo LoadBalancer, obter o endereço IP externo do serviço e abrir o serviço em um navegador. Certifique-se de executar este script em um ambiente que permita o acesso externo, como o Colab Notebook ou localmente em sua máquina.
