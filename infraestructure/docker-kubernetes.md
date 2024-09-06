# Docker e Kubernetes

## 1. O que é Docker?

É uma plataforma que permite criar, distribuir e executar aplicações em contêineres. Um **contêiner** é uma unidade leve e portável que contém tudo o que a aplicação precisa para funcionar: código, bibliotecas, dependências e configurações. Docker facilita a padronização de ambientes, garantindo que a aplicação funcione da mesma forma em diferentes sistemas, desde o desenvolvimento até a produção.

### 1.1. Conceitos Básicos

- **Imagem (Image):** É um "modelo" imutável que contém todos os arquivos necessários para executar a aplicação, como o código, o sistema operacional e as dependências. As imagens são criadas a partir de um arquivo chamado **Dockerfile**.
- **Contêiner (Container):** É uma instância em execução de uma imagem. Cada contêiner é isolado, mas pode se comunicar com outros contêineres, se necessário.
- **Dockerfile:** Um arquivo de texto que define os passos para criar uma imagem Docker. Nele, você especifica a base da imagem, as dependências, o código e as configurações.
- **Docker Hub:** Um repositório público onde você pode armazenar e compartilhar suas imagens Docker.

### 1.2. Vantagens do Docker

- **Portabilidade:** Funciona da mesma forma em diferentes ambientes (local, teste, produção).
- **Eficiência:** Menos pesado que uma máquina virtual, pois compartilha o mesmo kernel do sistema operacional.
- **Escalabilidade:** Facilita a criação de múltiplas instâncias da aplicação.

### 1.3. Exemplo de Dockerfile

```
# Use uma imagem base oficial do Node.js
FROM node:14

# Defina o diretório de trabalho no contêiner
WORKDIR /app

# Copie o package.json e instale as dependências
COPY package.json .
RUN npm install

# Copie o código fonte da aplicação
COPY . .

# Exponha a porta em que a aplicação será executada
EXPOSE 3000

# Comando para iniciar a aplicação
CMD ["npm", "start"]

```

### 1.4. Principais Comandos Docker

- `docker build`: Cria uma imagem a partir de um Dockerfile.
- `docker run`: Executa um contêiner baseado em uma imagem.
- `docker ps`: Lista os contêineres em execução.
- `docker stop`: Para a execução de um contêiner.
- `docker exec`: Executa comandos dentro de um contêiner em execução.

## 2. O que é Kubernetes?

**Kubernetes** é uma plataforma de orquestração de contêineres que automatiza o gerenciamento, o escalonamento e a implantação de aplicações em contêineres. Ele permite que você organize seus contêineres em clusters, facilitando o gerenciamento de ambientes distribuídos e complexos.

### 2.1. Conceitos Básicos

- **Cluster:** Um grupo de servidores (nós) que executam os contêineres. Kubernetes gerencia o cluster, distribuindo e escalonando os contêineres conforme necessário.
- **Pod:** A menor unidade executável em Kubernetes, que pode conter um ou mais contêineres. Todos os contêineres dentro de um Pod compartilham o mesmo armazenamento e rede.
- **Node:** Um servidor físico ou virtual que faz parte do cluster Kubernetes. Ele executa os contêineres que compõem sua aplicação.
- **Deployment:** Um objeto Kubernetes que gerencia a implantação de Pods, garantindo que o número correto de réplicas esteja sempre em execução.
- **Service:** Um objeto que expõe os Pods para a rede, permitindo que outros serviços ou usuários se conectem aos contêineres executados no Kubernetes.

### 2.2. Vantagens do Kubernetes

- **Automatização:** Kubernetes automatiza o escalonamento, balanceamento de carga, recuperação de falhas e implantação de contêineres.
- **Escalabilidade:** Facilita o escalonamento de aplicações para lidar com maior carga, distribuindo contêineres em vários nós.
- **Recuperação de Falhas:** Monitora a saúde dos contêineres e substitui automaticamente os que falham.

### 2.3. Arquitetura Kubernetes

- **Control Plane (Plano de Controle):** É o cérebro do Kubernetes, responsável por gerenciar o cluster, planejar onde os Pods devem ser executados e manter o estado desejado.
  - Componentes principais: `kube-apiserver`, `etcd`, `kube-scheduler`, `kube-controller-manager`.
- **Worker Nodes (Nós de Trabalho):** Executam os contêineres (Pods) e se comunicam com o Plano de Controle para receber instruções.
  - Componentes principais: `kubelet`, `kube-proxy`, `Container Runtime`.

### 2.4. Exemplo de Manifesto Kubernetes (Deployment)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: exemplo-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: exemplo-app
  template:
    metadata:
      labels:
        app: exemplo-app
    spec:
      containers:
      - name: exemplo-container
        image: nginx:latest
        ports:
        - containerPort: 80

```

Esse exemplo define um deployment de 3 réplicas de um contêiner rodando a imagem do Nginx.

### 2.5. Principais Comandos Kubernetes

- `kubectl apply -f`: Aplica as configurações definidas em um arquivo YAML.
- `kubectl get pods`: Lista todos os Pods no cluster.
- `kubectl describe pod`: Mostra detalhes de um Pod específico.
- `kubectl logs`: Exibe os logs de um contêiner em execução.
- `kubectl scale`: Escalona um deployment para mais ou menos réplicas.

## 3. Diferenças Entre Docker e Kubernetes

- **Escopo:** Docker é uma ferramenta para criar e executar contêineres. Kubernetes é uma plataforma de orquestração para gerenciar múltiplos contêineres em produção.
- **Isolamento vs. Orquestração:** Docker foca no isolamento de aplicações em contêineres, enquanto Kubernetes foca na orquestração desses contêineres, escalando e distribuindo-os automaticamente.
- **Gerenciamento:** Docker Swarm é uma ferramenta básica de orquestração integrada ao Docker, enquanto Kubernetes é uma solução mais robusta, projetada para ambientes mais complexos.

## 4. Quando Usar Docker e Kubernetes?

### 4.1. Docker

- **Desenvolvimento Local:** Docker é ideal para desenvolver, testar e executar aplicativos em contêineres localmente.
- **Ambientes Pequenos:** Para pequenos ambientes onde você não precisa de gerenciamento avançado de contêineres, Docker sozinho pode ser suficiente.

### 4.2. Kubernetes

- **Ambientes Complexos:** Kubernetes é recomendado para aplicações que precisam de alta disponibilidade, escalabilidade automática e orquestração de múltiplos contêineres.
- **Produção em Grande Escala:** Se você precisa gerenciar centenas ou milhares de contêineres distribuídos em vários servidores, Kubernetes é uma escolha adequada.

## 5. Exemplo Prático: Docker e Kubernetes Juntos

Vamos considerar um cenário onde você desenvolve uma aplicação usando Docker, e depois a coloca em produção com Kubernetes.

### 5.1. Desenvolvimento com Docker

1. Escreva o código da aplicação e crie um `Dockerfile`.
2. Use o Docker para construir a imagem e testar o contêiner localmente.

    ```bash
    docker build -t minha-app .
    docker run -p 3000:3000 minha-app
    ```

### 5.2. Implantação com Kubernetes

1. Escreva um manifesto YAML para criar um Deployment em Kubernetes.
2. Aplique o manifesto ao cluster Kubernetes.

    ```bash
    kubectl apply -f deployment.yaml
    ```

3. Use Kubernetes para escalar a aplicação conforme necessário.

    ```bash
    kubectl scale deployment/minha-app --replicas=5
    ```

## 6. Desafios Comuns e Boas Práticas

### 6.1. Desafios Comuns

- **Configuração Inicial de Kubernetes:** Kubernetes pode ser complicado para configurar inicialmente, especialmente para quem é novo em orquestração de contêineres.
- **Gerenciamento de Estado:** Aplicações stateful (com estado) são mais complexas de gerenciar em contêineres e precisam de volumes persistentes.
- **Segurança:** Gerenciar a segurança de contêineres em produção requer atenção especial para evitar vulnerabilidades.

### 6.2. Boas Práticas

- **Mantenha Contêineres Leves:** Imagens Docker devem ser pequenas e conter apenas o necessário para executar a aplicação.
- **Automatize Tudo:** Use pipelines de CI/CD para construir, testar e implantar contêineres automaticamente.
- **Monitore Contêineres e Pods:** Utilize ferramentas como Prometheus e Grafana para monitorar o desempenho e a saúde dos contêineres e do cluster Kubernetes.

## 7. Conclusão

**Docker** e **Kubernetes** são ferramentas poderosas para construir, implantar e gerenciar aplicações em contêineres. Docker simplifica o desenvolvimento e a padronização de ambientes, enquanto Kubernetes ajuda a escalar e orquestrar essas aplicações em ambientes de produção complexos. Entender como usar essas tecnologias permitirá que você crie sistemas mais flexíveis, escaláveis e resilientes.
