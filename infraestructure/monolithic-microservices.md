# Monolithic e Microservices

## 1. O que é um Monolithic?

É uma arquitetura de software onde todas as funcionalidades e componentes de uma aplicação estão unificados em um único código-fonte. Em outras palavras, toda a lógica da aplicação (como autenticação, processamento de pedidos, envio de emails, etc.) está centralizada em uma única base de código e geralmente implantada como uma única unidade executável.

### 1.1 Características de um Monolithic

- **Código Unificado:** Toda a lógica da aplicação está em um único repositório e é compilada ou construída em uma única aplicação.
- **Deploy Conjunto:** Qualquer alteração no código exige o redeploy de toda a aplicação.
- **Interdependência:** Os componentes são fortemente acoplados, ou seja, mudanças em uma parte podem afetar outras.

### 1.2 Vantagens

- **Simples para Desenvolver:** No início, é mais fácil construir e gerenciar.
- **Teste e Deploy Simples:** Como tudo está em um só lugar, é mais fácil testar e fazer o deploy.
- **Facilidade de Gerenciamento:** Uma única base de código é mais simples para pequenas equipes de desenvolvimento.

### 1.3 Desvantagens

- **Escalabilidade Limitada:** Escalar uma parte específica da aplicação é difícil, já que toda a aplicação precisa ser escalada.
- **Dificuldade de Manutenção:** À medida que o código cresce, o Monolithic pode se tornar difícil de manter e compreender.
- **Tempo de Deploy Longo:** Alterações menores ainda exigem o redeploy de toda a aplicação.

## 2. O que são Microservices?

São uma arquitetura onde a aplicação é dividida em serviços menores e independentes, cada um responsável por uma funcionalidade específica. Esses serviços se comunicam entre si, geralmente através de APIs ou Queue Services, e podem ser desenvolvidos, implantados e escalados de forma independente.

### 2.1 Características de Microservices

- **Serviços Independentes:** Cada serviço tem seu próprio código-fonte, banco de dados e ciclo de vida.
- **Comunicação por API:** Os microservices geralmente se comunicam através de APIs REST, RPC, ou mensagens assíncronas (queue service).
- **Desenvolvimento e Deploy Independentes:** Cada serviço pode ser desenvolvido, testado e implantado independentemente dos outros.

### 2.2 Vantagens

- **Escalabilidade Específica:** Serviços individuais podem ser escalados conforme a necessidade, sem afetar o restante da aplicação.
- **Facilidade de Manutenção:** Com serviços menores e mais focados, o código é mais fácil de gerenciar e manter.
- **Autonomia das Equipes:** Equipes podem trabalhar de forma independente em diferentes serviços, sem precisar coordenar todos os detalhes com outras equipes.

### 2.3 Desvantagens

- **Complexidade Operacional:** Gerenciar múltiplos serviços pode ser desafiador, especialmente em termos de comunicação, autenticação e monitoramento.
- **Latência de Comunicação:** A comunicação entre serviços pode adicionar latência, especialmente se os serviços estiverem distribuídos geograficamente.
- **Desenvolvimento Distribuído:** Desenvolver e testar aplicações distribuídas requer ferramentas e processos mais avançados.

## 3. Comparação entre Monolithicic e Microservices

| Aspecto | Monolithic | Microservices |
| --- | --- | --- |
| **Arquitetura** | Centralizada, única aplicação | Distribuída, vários serviços menores |
| **Escalabilidade** | Escalada como um todo | Escalada independente de cada serviço |
| **Desenvolvimento** | Código unificado, desenvolvimento conjunto | Desenvolvimento paralelo e independente |
| **Manutenção** | Mais difícil à medida que cresce | Mais fácil devido ao escopo reduzido de cada serviço |
| **Complexidade** | Simples no início, pode complicar com o tempo | Complexidade maior desde o início |
| **Deploy** | Toda a aplicação precisa ser redeployada | Cada serviço pode ser implantado separadamente |

## 4. Exemplos Práticos

### 4.1 Exemplo de Monolithic (Java Spring Boot)

Aqui está um exemplo de uma aplicação simples de e-commerce construída como um Monolithic usando Spring Boot:

```java
@SpringBootApplication
public class ECommerceApplication {

    public static void main(String[] args) {
        SpringApplication.run(ECommerceApplication.class, args);
    }

    // Controlador para gerenciar produtos
    @RestController
    @RequestMapping("/products")
    public class ProductController {

        @GetMapping
        public List<Product> getAllProducts() {
            // lógica para retornar todos os produtos
        }

        @PostMapping
        public void addProduct(@RequestBody Product product) {
            // lógica para adicionar um novo produto
        }
    }

    @RestController
    @RequestMapping("/orders")
    public class OrderController {

        @PostMapping
        public void createOrder(@RequestBody Order order) {
            // lógica para criar um novo pedido
        }
    }
}

```

### 4.2 Exemplo de Microservices (Java Spring Boot)

Agora, a mesma aplicação e-commerce pode ser dividida em microservices. Por exemplo, poderíamos ter um microservice para **Produtos** e outro para **Pedidos**.

**Serviço de Produtos:**

```java
@SpringBootApplication
public class ProductServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(ProductServiceApplication.class, args);
    }

    @RestController
    @RequestMapping("/products")
    public class ProductController {

        @GetMapping
        public List<Product> getAllProducts() {
            // lógica para retornar todos os produtos
        }

        @PostMapping
        public void addProduct(@RequestBody Product product) {
            // lógica para adicionar um novo produto
        }
    }
}

```

**Serviço de Pedidos:**

```java
@SpringBootApplication
public class OrderServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }

    @RestController
    @RequestMapping("/orders")
    public class OrderController {

        @PostMapping
        public void createOrder(@RequestBody Order order) {
            // lógica para criar um novo pedido
        }
    }
}

```

Aqui, cada microservice (Produto e Pedido) é implantado separadamente e pode ser escalado individualmente.

## 5. Quando Usar Monolithicic e Microservices?

- **Monolithicic:**
  - Projetos pequenos ou de curto prazo.
  - Quando o time é pequeno e precisa de simplicidade no desenvolvimento.
  - Quando a aplicação não precisa ser escalada de forma significativa.
- **Microservices:**
  - Projetos grandes e de longo prazo que exigem escalabilidade.
  - Quando há múltiplos times trabalhando em partes diferentes da aplicação.
  - Quando é necessário desenvolver e implantar funcionalidades de forma independente.

## 6. Conclusão

A escolha entre **Monolithic** e **Microservices** depende do tamanho do projeto, das necessidades de escalabilidade e da estrutura do time. Monolithicic são mais simples de iniciar, mas podem se tornar difíceis de manter à medida que a aplicação cresce. Microservices trazem maior flexibilidade e escalabilidade, mas exigem mais esforço de gerenciamento e monitoramento.
