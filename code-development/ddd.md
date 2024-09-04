# Domain-Driven Design (DDD)

## 1. O que é Domain-Driven Design (DDD)?

É uma abordagem para o desenvolvimento de software que foca no domínio central do negócio. Foi introduzida por Eric Evans em seu livro *Domain-Driven Design: Tackling Complexity in the Heart of Software*. O DDD promove a criação de software baseado em modelos que refletem o domínio de negócio, e busca uma colaboração próxima entre desenvolvedores e especialistas no negócio (domain experts).

A ideia principal do DDD é alinhar o código com o negócio, modelando o software de acordo com as regras e processos do domínio. Isso ajuda a garantir que o software resolva problemas reais e seja flexível o suficiente para evoluir à medida que o negócio cresce.

## 2. Benefícios do Domain-Driven Design

- **Alinhamento entre Tecnologia e Negócio:** O DDD aproxima desenvolvedores e especialistas no negócio, garantindo que o software reflete as necessidades do domínio.
- **Modelagem mais precisa:** Modelos baseados no domínio ajudam a capturar a lógica de negócio de forma clara e precisa.
- **Flexibilidade e Evolução:** Como o foco está no domínio, o código é mais adaptável às mudanças de requisitos e evolução do negócio.
- **Simplicidade para Domínios Complexos:** Em sistemas complexos, o DDD ajuda a gerenciar a complexidade dividindo o sistema em contextos menores e gerenciáveis.

## 3. Princípios Fundamentais do Domain-Driven Design

### 3.1. Domínio (Domain)

O **domínio** é o problema que o software está tentando resolver. É o "mundo real" da aplicação. Por exemplo, em um sistema de e-commerce, o domínio inclui conceitos como **clientes**, **pedidos**, **produtos**, e **pagamentos**. O foco do DDD é capturar o domínio corretamente no software.

### 3.2. Especialistas do Domínio (Domain Experts)

**Especialistas do Domínio** são as pessoas que conhecem profundamente o negócio e o domínio específico que o software está tentando resolver. Esses especialistas colaboram com os desenvolvedores para definir as regras, processos e conceitos do domínio que devem ser representados no código.

### 3.3. Linguagem Ubíqua (Ubiquitous Language)

A **linguagem ubíqua** é uma linguagem comum usada por desenvolvedores e especialistas do domínio para descrever o sistema. Essa linguagem é compartilhada por toda a equipe e deve ser usada consistentemente no código, nas conversas e na documentação. Isso evita mal-entendidos e alinha o desenvolvimento com o negócio.

**Exemplo:**
Se o termo "Pedido" é usado no negócio, ele também deve ser usado no código, em vez de termos diferentes como "Ordem" ou "Compra".

### 3.4. Contextos Delimitados (Bounded Contexts)

**Contextos Delimitados** são subdomínios ou áreas dentro do domínio maior. Cada contexto delimitado tem seu próprio modelo e linguagem ubíqua, o que ajuda a gerenciar a complexidade em sistemas grandes. Um contexto delimitado é uma fronteira lógica que define onde um determinado conjunto de regras e conceitos se aplica.

**Exemplo:**
Em um sistema de e-commerce, você pode ter um contexto delimitado para "Vendas" e outro para "Inventário". Cada um desses contextos lida com aspectos diferentes do domínio, com suas próprias regras.

### 3.5. Entidades e Value Objects

- **Entidades:** São objetos que possuem uma identidade única que persiste ao longo do tempo, mesmo que seu estado mude. Elas representam conceitos importantes no domínio que têm vida útil própria e precisam ser rastreados.

    **Exemplo:** Em um sistema de e-commerce, um `Cliente` pode ser uma entidade, pois tem uma identidade única (como um ID) e seu estado (nome, endereço) pode mudar ao longo do tempo.

- **Value Objects (Objetos de Valor):** São objetos que não têm identidade própria, mas sim valor. Eles são imutáveis e usados para descrever atributos de Entidades. Dois objetos de valor são considerados iguais se seus atributos são iguais.

    **Exemplo:** Um `Endereço` pode ser um objeto de valor, pois é composto por atributos como rua e número, mas não tem identidade própria.

### 3.6. Agregados (Aggregates)

Um **agregado** é um conjunto de objetos (entidades e objetos de valor) que estão logicamente relacionados e formam uma unidade de consistência. O agregado tem uma raiz (entidade raiz) que gerencia as alterações e protege a integridade de seus componentes.

**Exemplo:**
Em um sistema de pedidos, o agregado `Pedido` pode conter a entidade raiz `Pedido` e várias entidades `ItemPedido`. Todas as operações no agregado devem passar pela entidade raiz `Pedido`.

### 3.7. Repositórios (Repositories)

**Repositórios** são interfaces que fornecem acesso a coleções de entidades agregadas. Eles abstraem os detalhes de armazenamento e recuperação de dados, permitindo que a lógica de negócio trabalhe com objetos de domínio sem se preocupar com a persistência.

**Exemplo:**

```java
public interface RepositorioPedido {
    Pedido obterPorId(int id);
    void salvar(Pedido pedido);
}

```

### 3.8. Serviços de Domínio (Domain Services)

**Serviços de Domínio** são usados para operações que não pertencem a uma entidade ou objeto de valor específico, mas que são relevantes para o domínio. Eles encapsulam lógica de negócio que envolve várias entidades ou que não se encaixa naturalmente em uma única entidade.

**Exemplo:**
Um serviço de "Cálculo de Frete" pode ser um serviço de domínio, já que o cálculo pode depender de várias entidades, como `Pedido`, `Endereço`, e `Transportadora`.

## 4. Exemplo Prático de Domain-Driven Design

Vamos aplicar os conceitos de DDD em um exemplo simples de um sistema de pedidos.

### 4.1. Entidades e Value Objects

```java
// Entidade Cliente
public class Cliente {
    private final String id;
    private String nome;
    private Endereco endereco;

    public Cliente(String id, String nome, Endereco endereco) {
        this.id = id;
        this.nome = nome;
        this.endereco = endereco;
    }

    // Getters e setters
}

// Value Object Endereco
public class Endereco {
    private final String rua;
    private final String cidade;

    public Endereco(String rua, String cidade) {
        this.rua = rua;
        this.cidade = cidade;
    }

    // Getters, equals, hashCode (imutável)
}

```

### 4.2. Agregado Pedido

```java
// Entidade Raiz do Agregado
public class Pedido {
    private final String id;
    private final Cliente cliente;
    private final List<ItemPedido> itens;

    public Pedido(String id, Cliente cliente) {
        this.id = id;
        this.cliente = cliente;
        this.itens = new ArrayList<>();
    }

    public void adicionarItem(ItemPedido item) {
        itens.add(item);
    }

    // Métodos de negócio, getters
}

```

### 4.3. Repositório

```java
public interface RepositorioPedido {
    Pedido obterPorId(String id);
    void salvar(Pedido pedido);
}

```

## 5. Desafios do Domain-Driven Design

- **Curva de Aprendizado:** DDD pode ser desafiador para desenvolvedores juniores, especialmente em projetos pequenos ou com pouca complexidade, onde o overhead da modelagem complexa pode não valer a pena.
- **Complexidade Inicial:** A aplicação de DDD pode adicionar complexidade ao design do sistema, especialmente ao dividir o domínio em múltiplos contextos delimitados.
- **Colaboração com Especialistas no Domínio:** Para aplicar DDD com sucesso, é essencial a colaboração contínua com especialistas no domínio para garantir que o modelo reflete corretamente as regras e processos do negócio.

## 6. Quando Usar Domain-Driven Design

DDD é mais apropriado para sistemas complexos, onde a lógica de negócio é central e muda frequentemente. Para sistemas pequenos ou com lógica simples, o DDD pode ser um exagero e adicionar complexidade desnecessária.

## 7. Conclusão

**Domain-Driven Design** é uma abordagem poderosa para o desenvolvimento de software em domínios complexos. Ao focar no domínio e usar uma linguagem ubíqua compartilhada, o DDD ajuda a garantir que o software reflete com precisão as regras e processos do negócio. Embora tenha uma curva de aprendizado, aplicar DDD em sistemas complexos pode resultar em código mais claro, flexível e adaptável às mudanças.

Pratique DDD em projetos menores, trabalhando com especialistas no domínio e aplicando os conceitos gradualmente. Ao dominar DDD, você será capaz de enfrentar desafios de software mais complexos e criar sistemas que realmente atendem às necessidades do negócio.