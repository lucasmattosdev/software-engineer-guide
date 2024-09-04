# Clean Architecture

## 1. O que é Clean Architecture?

É um conjunto de princípios arquiteturais que promovem a separação de responsabilidades, tornando o sistema mais modular, fácil de testar, e de manter ao longo do tempo. Criada por Robert C. Martin (Uncle Bob), a Clean Architecture é uma evolução de conceitos anteriores, como a Arquitetura em Camadas (Layered Architecture), Arquitetura Hexagonal (Hexagonal Architecture), e DDD (Domain-Driven Design).

O objetivo da Clean Architecture é garantir que o sistema seja **independente de frameworks, UI, banco de dados, e qualquer detalhe externo**, focando no domínio e na lógica de negócio da aplicação.

## 2. Benefícios da Clean Architecture

- **Manutenibilidade:** Ao isolar as responsabilidades em camadas, torna-se mais fácil fazer mudanças sem impactar outras partes do sistema.
- **Testabilidade:** Como a lógica de negócio é independente de detalhes externos, fica mais fácil escrever testes unitários e de integração.
- **Escalabilidade:** Facilita a adição de novas funcionalidades sem comprometer o design existente.
- **Flexibilidade:** Permite substituir detalhes externos (como frameworks ou bancos de dados) sem afetar o núcleo da aplicação.

## 3. Estrutura Básica da Clean Architecture

A Clean Architecture pode ser visualizada como um conjunto de círculos concêntricos, onde cada círculo representa uma camada com diferentes níveis de abstração. A principal regra é que **dependências só podem apontar para dentro**, ou seja, camadas externas podem depender de camadas internas, mas o contrário não é permitido.

### 3.1. Camadas da Clean Architecture

1. **Entities (Entidades):**
    - Localizadas no centro da arquitetura, as Entidades representam os objetos de negócio mais fundamentais. Elas contêm a lógica de negócio central e são independentes de frameworks, UI, e banco de dados.
    - Exemplo: Classes de domínio que modelam os conceitos principais do negócio, como `Cliente`, `Pedido`, etc.
2. **Use Cases (Casos de Uso):**
    - Os Casos de Uso definem as regras de negócio específicas da aplicação, coordenando o fluxo de dados entre as Entidades e os sistemas externos. Eles contêm a lógica de negócio específica de cada funcionalidade.
    - Exemplo: Um caso de uso pode ser o processo de criação de um pedido, validando dados e interagindo com o repositório de dados.
3. **Interface Adapters (Adaptadores de Interface):**
    - Esta camada contém adaptadores que convertem os dados de um formato que o sistema pode processar para outro que as camadas externas (como UI ou banco de dados) possam entender. Isso inclui controladores, gateways, e presenters.
    - Exemplo: Controladores de API, classes de mapeamento de dados, e adaptadores de repositório.
4. **Frameworks and Drivers (Frameworks e Drivers):**
    - A camada mais externa, onde estão os detalhes técnicos da aplicação, como frameworks, bibliotecas, e o acesso ao banco de dados. O objetivo é que esses detalhes possam ser substituídos sem afetar o restante da aplicação.
    - Exemplo: Frameworks web (como Spring, Django), bancos de dados, e bibliotecas de autenticação.

## 4. Princípios Fundamentais da Clean Architecture

### 4.1. Dependência Reversa (Dependency Inversion)

Na Clean Architecture, o fluxo de dependências é invertido. As camadas internas (como Entidades e Casos de Uso) não dependem diretamente de detalhes externos (como banco de dados ou UI). Em vez disso, as camadas externas dependem das internas. Isso é obtido através de abstrações e injeção de dependência.

**Exemplo:**

```java
// Interface no domínio
public interface RepositorioCliente {
    Cliente obterClientePorId(int id);
}

// Implementação no adaptador externo
public class RepositorioClienteImpl implements RepositorioCliente {
    // Acesso ao banco de dados
}

```

### 4.2. Independência de Frameworks

Frameworks devem ser usados como ferramentas, não como a base da sua aplicação. A lógica de negócio não deve depender diretamente de nenhum framework, permitindo que o framework seja substituído, se necessário, sem grandes mudanças no código.

**Exemplo:**
Em vez de basear toda a sua lógica de negócio em anotações de frameworks como Spring, isole a lógica de negócio em classes que não têm dependências externas.

### 4.3. Testabilidade

Como as camadas internas (Entidades e Casos de Uso) são isoladas de detalhes externos, fica mais fácil testar essas camadas sem a necessidade de simular bancos de dados, APIs externas, ou frameworks. Isso permite escrever testes unitários simples e rápidos.

### 4.4. Interface Explícita

Cada camada deve se comunicar com a próxima camada através de interfaces explícitas, facilitando a substituição de implementações e a definição clara das responsabilidades.

## 5. Exemplo Prático de Clean Architecture

Vamos implementar um exemplo simples de um sistema de pedidos usando a Clean Architecture em Java.

### 5.1. Entidades

```java
public class Pedido {
    private int id;
    private String cliente;
    private List<Item> itens;

    public Pedido(int id, String cliente) {
        this.id = id;
        this.cliente = cliente;
        this.itens = new ArrayList<>();
    }

    public void adicionarItem(Item item) {
        itens.add(item);
    }

    // Getters e lógica de negócio
}

```

### 5.2. Casos de Uso

```java
public class CriarPedidoUseCase {

    private final RepositorioPedido repositorioPedido;

    public CriarPedidoUseCase(RepositorioPedido repositorioPedido) {
        this.repositorioPedido = repositorioPedido;
    }

    public void executar(Pedido pedido) {
        // Regras de negócio para criar um pedido
        repositorioPedido.salvar(pedido);
    }
}

```

### 5.3. Adaptadores de Interface

```java
public class PedidoController {

    private final CriarPedidoUseCase criarPedidoUseCase;

    public PedidoController(CriarPedidoUseCase criarPedidoUseCase) {
        this.criarPedidoUseCase = criarPedidoUseCase;
    }

    public void criarPedido(PedidoDTO pedidoDTO) {
        Pedido pedido = new Pedido(pedidoDTO.getId(), pedidoDTO.getCliente());
        // Mapeamento dos dados e chamada do caso de uso
        criarPedidoUseCase.executar(pedido);
    }
}

```

### 5.4. Frameworks e Drivers

Aqui, você implementaria a interface `RepositorioPedido` usando o banco de dados ou outro serviço externo de sua escolha. A camada de frameworks seria responsável por configurar o ambiente, mas sem afetar as camadas internas.

```java
public class RepositorioPedidoImpl implements RepositorioPedido {
    // Implementação de acesso ao banco de dados
}

```

## 6. Desafios da Clean Architecture

- **Curva de Aprendizado:** Para desenvolvedores juniores, entender e aplicar todos os princípios pode ser desafiador no início. É importante praticar e aplicar Clean Architecture em pequenos projetos para se familiarizar com os conceitos.
- **Complexidade Inicial:** A configuração inicial pode parecer mais complexa devido à criação de interfaces e camadas adicionais, mas isso compensa na manutenibilidade e flexibilidade a longo prazo.
- **Consistência:** Para obter os benefícios da Clean Architecture, é importante que toda a equipe siga os mesmos princípios e práticas, garantindo a consistência do código ao longo do tempo.

## 7. Conclusão

**Clean Architecture** é uma abordagem poderosa para criar sistemas robustos, modulares e fáceis de manter. Ao aplicar seus princípios, você torna seu código mais flexível, testável e independente de detalhes externos, como frameworks e bancos de dados. Embora a curva de aprendizado possa ser desafiadora para desenvolvedores juniores, dominar esses conceitos será um grande passo para se tornar um desenvolvedor mais eficiente e capaz de lidar com sistemas complexos.

Pratique a Clean Architecture em projetos pequenos e vá aplicando os conceitos gradualmente em sistemas maiores. Essa abordagem vai melhorar a qualidade e a manutenibilidade do seu código a longo prazo.
