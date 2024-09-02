# SOLID Principles

## 1. O que são os Princípios SOLID?

É um acrônimo que descreve cinco princípios fundamentais para o design de software orientado a objetos. Esses princípios foram definidos por Robert C. Martin (conhecido como Uncle Bob) e têm como objetivo criar sistemas mais flexíveis, modulares e fáceis de manter.

Os cinco princípios são:

1. **S** - Single Responsibility Principle (Princípio da Responsabilidade Única)
2. **O** - Open/Closed Principle (Princípio Aberto/Fechado)
3. **L** - Liskov Substitution Principle (Princípio da Substituição de Liskov)
4. **I** - Interface Segregation Principle (Princípio da Segregação de Interfaces)
5. **D** - Dependency Inversion Principle (Princípio da Inversão de Dependência)

## 2. Por que Usar os Princípios SOLID?

- **Facilita a Manutenção:** O código se torna mais modular, o que facilita futuras alterações e melhorias.
- **Reduz o Acoplamento:** Ajuda a criar classes e módulos independentes, o que diminui o impacto de mudanças.
- **Aumenta a Reutilização:** As funcionalidades são separadas em pequenas partes que podem ser reutilizadas em diferentes contextos.
- **Melhora a Testabilidade:** Código mais desacoplado e organizado é mais fácil de testar.

## 3. Os Princípios SOLID

### 3.1. Single Responsibility Principle (SRP)

**Princípio da Responsabilidade Única**

Cada classe deve ter **uma única responsabilidade** ou motivo para mudar. Isso significa que uma classe deve fazer apenas uma coisa e fazer bem.

**Exemplo:** Suponha que você tenha uma classe que lida com operações de persistência de dados e lógica de negócios ao mesmo tempo. De acordo com o SRP, essas responsabilidades devem ser separadas em diferentes classes.

```java
class Pedido {
    public void calcularTotal() {
        // Lógica de negócios para calcular o total
    }
}

class PedidoRepository {
    public void salvarPedido(Pedido pedido) {
        // Código para salvar o pedido no banco de dados
    }
}

```

#### Quando Usar

- Sempre que perceber que uma classe está fazendo mais de uma coisa, divida as responsabilidades em diferentes classes.

### 3.2. Open/Closed Principle (OCP)

**Princípio Aberto/Fechado**

Classes, módulos e funções devem estar **abertos para extensão, mas fechados para modificação**. Isso significa que devemos ser capazes de estender o comportamento de uma classe sem alterar seu código-fonte original.

**Exemplo:** Vamos supor que você tem uma classe que calcula descontos. Em vez de modificar a classe existente para adicionar novos tipos de descontos, você pode estender a classe original.

```java
abstract class Desconto {
    abstract double aplicarDesconto(double valor);
}

class DescontoFidelidade extends Desconto {
    @Override
    double aplicarDesconto(double valor) {
        return valor * 0.9; // 10% de desconto
    }
}

class DescontoPromocional extends Desconto {
    @Override
    double aplicarDesconto(double valor) {
        return valor * 0.8; // 20% de desconto
    }
}

```

#### Quando Usar

- Ao precisar adicionar novas funcionalidades sem alterar o código existente, use herança ou composição para estender o comportamento.

### 3.3. Liskov Substitution Principle (LSP)

**Princípio da Substituição de Liskov**

Objetos de uma classe derivada devem ser substituíveis por objetos de sua classe base **sem alterar o comportamento esperado do programa**. Ou seja, uma subclasse deve ser capaz de substituir sua classe base sem causar problemas.

**Exemplo:** Se você tem uma classe `Ave` e cria subclasses como `Pinguim`, o comportamento da subclasse não deve quebrar o contrato estabelecido pela classe base.

```java
class Ave {
    public void voar() {
        // Lógica para voar
    }
}

class Pinguim extends Ave {
    @Override
    public void voar() {
        // Pinguins não voam, o que quebra o princípio
        throw new UnsupportedOperationException("Pinguins não podem voar!");
    }
}

```

Nesse caso, o design precisa ser revisto para que subclasses que não podem voar não herdem um comportamento inadequado.

#### Quando Usar

- Sempre que criar subclasses, garanta que elas podem substituir a classe base sem alterar o comportamento esperado.

### 3.4. Interface Segregation Principle (ISP)

**Princípio da Segregação de Interfaces**

Uma classe não deve ser obrigada a implementar interfaces que não utiliza. Em vez de ter uma única interface grande, divida-a em interfaces menores e mais específicas.

**Exemplo:** Em vez de ter uma interface com muitos métodos que nem todas as classes implementariam, separe-a em várias interfaces menores.

```java
interface Ave {
    void bicar();
}

interface AveQueVoa extends Ave {
    void voar();
}

class Pato implements AveQueVoa {
    @Override
    public void bicar() {
        // Pato pode bicar
    }

    @Override
    public void voar() {
        // Pato pode voar
    }
}

class Galinha implements Ave {
    @Override
    public void bicar() {
        // Galinha pode bicar
    }
}

```

#### Quando Usar

- Quando perceber que diferentes implementações de uma interface não utilizam todos os métodos definidos, divida a interface em interfaces menores.

### 3.5. Dependency Inversion Principle (DIP)

**Princípio da Inversão de Dependência**

Dependa de **abstrações** e não de implementações concretas. Classes de alto nível não devem depender de classes de baixo nível; ambas devem depender de abstrações (interfaces ou classes abstratas).

**Exemplo:** Em vez de uma classe de alto nível instanciar diretamente uma classe de baixo nível, use injeção de dependência para passar a implementação.

```java
interface Repositorio {
    void salvar(Pedido pedido);
}

class PedidoRepository implements Repositorio {
    @Override
    public void salvar(Pedido pedido) {
        // Código para salvar o pedido no banco de dados
    }
}

class PedidoService {
    private Repositorio repositorio;

    public PedidoService(Repositorio repositorio) {
        this.repositorio = repositorio;
    }

    public void processarPedido(Pedido pedido) {
        repositorio.salvar(pedido);
    }
}

```

#### Quando Usar

- Sempre que for necessário utilizar uma implementação concreta em uma classe de alto nível, considere usar abstrações para melhorar a flexibilidade e testabilidade.

## 4. Benefícios de Usar SOLID

- **Código Mais Flexível e Reutilizável:** Os princípios SOLID ajudam a construir sistemas que podem crescer e evoluir sem exigir grandes refatorações.
- **Facilidade de Testes:** Com código mais modular e desacoplado, fica mais fácil testar individualmente cada componente do sistema.
- **Facilidade de Manutenção:** O código se torna mais legível, facilitando a manutenção e a adição de novas funcionalidades.

## 5. Quando Aplicar SOLID?

Os princípios SOLID são uma orientação importante, mas não devem ser aplicados cegamente. Em projetos pequenos ou protótipos, pode ser que você não precise aplicar todos esses princípios rigorosamente. No entanto, em sistemas maiores e que precisam ser escaláveis e fáceis de manter, o uso de SOLID ajuda a evitar problemas comuns de design e a manter a qualidade do código ao longo do tempo.

## 6. Conclusão

Os princípios SOLID fornecem diretrizes valiosas para escrever código orientado a objetos que seja robusto, flexível e fácil de manter. Ao entender e aplicar esses princípios, você será capaz de criar sistemas de software que resistem bem ao teste do tempo e às mudanças inevitáveis que virão com o crescimento do projeto.
