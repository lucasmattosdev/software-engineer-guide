# CI/CD

## 1. O que é CI/CD?

É um conjunto de práticas de automação que visam melhorar o desenvolvimento de software, integrando continuamente o código, testando-o automaticamente e implantando-o em produção de forma ágil e consistente. Essas práticas têm como objetivo acelerar o ciclo de vida do software, aumentando a eficiência do desenvolvimento e reduzindo o risco de erros.

- **Continuous Integration (CI):** Integração contínua é o processo de integrar código de diferentes desenvolvedores em um repositório compartilhado. Isso inclui a automação de testes e qualidade de código para garantir que o código recém-adicionado funcione corretamente com o código existente e esteja dentro dos padrões pré estabelecidos.
- **Continuous Delivery (CD - Entrega Contínua):** A entrega contínua automatiza a preparação do código para a produção. A ideia é que o código possa ser lançado a qualquer momento, já que ele passa por uma série de testes automatizados para garantir sua estabilidade.
- **Continuous Deployment (CD - Implantação Contínua):** A implantação contínua leva a entrega contínua um passo adiante. Aqui, o código que passa nos testes automatizados é implantado diretamente em produção sem intervenção manual.

## 2. Conceitos Básicos

- **Pipeline:** Um pipeline de CI/CD é uma série de etapas automatizadas que o código percorre, desde a integração até a entrega ou implantação. Ele normalmente inclui etapas como build (construção), testes, e implantação.
- **Build:** O processo de transformar o código-fonte em um aplicativo executável. Isso pode envolver compilação, empacotamento de bibliotecas e preparação para deploy.
- **Testes Automatizados:** São testes que verificam automaticamente se o código funciona como esperado. Eles podem ser unitários (testando componentes individuais) ou integrados (testando a interação entre diferentes partes do sistema).
- **Deploy:** O processo de implantar o código em um ambiente específico, como produção ou staging. Isso pode ser feito de forma manual ou automatizada, dependendo do nível de CD.

## 3. Como Funciona o CI/CD na Prática?

### 3.1. Continuous Integration (CI)

1. **Commit Frequente:** Os desenvolvedores fazem commits frequentes de seu código em um repositório compartilhado (como GitHub, GitLab, ou Bitbucket).
2. **Build Automatizado:** Cada vez que o código é enviado ao repositório, um servidor de CI (como Jenkins, CircleCI, ou GitLab CI) dispara um processo de build automatizado para garantir que o código possa ser compilado.
3. **Testes Automatizados:** Após o build, testes automatizados são executados para garantir que o código funcione conforme o esperado e não quebre funcionalidades existentes.

Exemplo de Pipeline Simples em YAML para GitLab CI:

```yaml
stages:
  - build
  - test

build:
  stage: build
  script:
    - echo "Construindo o projeto..."
    - ./gradlew build

test:
  stage: test
  script:
    - echo "Executando testes..."
    - ./gradlew test

```

### 3.2. Continuous Delivery (CD - Entrega Contínua)

1. **Deploy Automatizado para Ambientes de Teste/Staging:** Após o código passar nos testes, ele é automaticamente implantado em um ambiente de staging (pré-produção) para verificações manuais ou testes adicionais.
2. **Preparado para Produção:** O código está sempre pronto para ser implantado em produção a qualquer momento, mas a implantação em produção ainda pode exigir uma aprovação manual.

Exemplo de Pipeline de Entrega Contínua com Jenkins:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Staging Deploy') {
            steps {
                echo 'Deploying to staging environment...'
                // Script para deploy no ambiente de staging
            }
        }
    }
}

```

### 3.3. Continuous Deployment (CD - Implantação Contínua)

1. **Deploy Automatizado para Produção:** Aqui, após o código passar por todos os testes e validações, ele é automaticamente implantado em produção sem a necessidade de intervenção manual.
2. **Monitoramento Contínuo:** Após o deploy, o sistema deve ser monitorado automaticamente para garantir que tudo esteja funcionando corretamente em produção.

Exemplo de Pipeline com Implantação Contínua no CircleCI:

```yaml
version: 2.1

jobs:
  build:
    docker:
      - image: circleci/openjdk:11-jdk
    steps:
      - checkout
      - run: ./gradlew build

  test:
    docker:
      - image: circleci/openjdk:11-jdk
    steps:
      - run: ./gradlew test

  deploy:
    docker:
      - image: circleci/openjdk:11-jdk
    steps:
      - run: echo "Deploying to production..."
      # Script para deploy em produção

workflows:
  version: 2
  build_test_deploy:
    jobs:
      - build
      - test
      - deploy

```

## 4. Ferramentas Populares de CI/CD

- **Jenkins:** Um dos servidores de automação mais populares, altamente configurável e extensível com plugins.
- **GitLab CI:** Ferramenta integrada ao GitLab, que oferece pipelines de CI/CD fáceis de configurar usando arquivos YAML.
- **CircleCI:** Ferramenta de CI/CD que se integra bem com projetos baseados em contêineres.
- **Travis CI:** Popular para projetos open-source, simples de configurar para repositórios GitHub.
- **Azure DevOps:** Plataforma completa de DevOps da Microsoft com integração de CI/CD.
- **GitHub Actions:** CI/CD diretamente integrado ao GitHub com configurações em YAML.

## 5. Vantagens de CI/CD

- **Menor Risco:** Ao integrar o código continuamente e testar automaticamente, bugs e problemas são identificados mais cedo, antes de serem lançados em produção.
- **Agilidade:** O ciclo de desenvolvimento é acelerado, com releases mais frequentes e menores.
- **Qualidade do Software:** Testes automatizados garantem que o código está sempre funcionando conforme o esperado.
- **Colaboração:** A integração contínua facilita a colaboração entre equipes de desenvolvimento, já que todos trabalham em uma base de código unificada e sempre atualizada.

### 6. Desafios de Implementar CI/CD

- **Configuração Inicial:** Configurar pipelines e automatizar todos os processos pode exigir um investimento inicial significativo de tempo e esforço.
- **Cultura de Equipe:** Adotar CI/CD exige que as equipes mudem sua mentalidade para priorizar a automação e a entrega contínua de código de alta qualidade.
- **Monitoramento e Manutenção:** Uma vez que a implantação contínua está ativa, é essencial ter monitoramento adequado para detectar rapidamente problemas em produção.

## 7. Conclusão

O CI/CD é uma prática essencial para o desenvolvimento ágil e moderno, permitindo que as equipes de software entreguem valor de forma mais rápida e com maior confiança. Embora possa exigir uma curva de aprendizado inicial, a automação de testes, builds e deploys resulta em maior eficiência e qualidade de software a longo prazo.
