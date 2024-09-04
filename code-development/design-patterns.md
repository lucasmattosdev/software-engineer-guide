# Design Patterns

## 1. O que são Design Patterns?

São soluções recorrentes para problemas comuns no design de software. Eles são como modelos pré-definidos que podem ser aplicados a problemas específicos de forma a melhorar a estrutura e a qualidade do código. Esses padrões não são código pronto para copiar e colar, mas sim guias para a construção de soluções robustas, reutilizáveis e escaláveis.

## 2. Por que Usar Design Patterns?

- **Reutilização de Código:** Ajuda a evitar a reinvenção da roda, reutilizando soluções comprovadas.
- **Manutenibilidade:** Facilita a manutenção e a evolução do código ao longo do tempo.
- **Compreensão e Comunicação:** Fornece uma linguagem comum para que os desenvolvedores discutam e comuniquem soluções de design de maneira eficaz.
- **Flexibilidade:** Muitos padrões permitem que o código seja mais flexível e menos dependente de implementações específicas.

## 3. Categorias de Design Patterns

Os padrões de projeto geralmente são divididos em três categorias principais:

1. **Padrões Criacionais:** Envolvem a criação de objetos e ajudam a desacoplar o sistema da forma como os objetos são criados.
2. **Padrões Estruturais:** Tratam da composição de classes e objetos para formar estruturas maiores e mais complexas.
3. **Padrões Comportamentais:** Se concentram em como as classes e objetos interagem entre si, facilitando a comunicação e o fluxo de dados.

## 4. Exemplos de Design Patterns

### 4.1. Padrões Criacionais

#### 4.1.1. Singleton

O padrão **Singleton** garante que uma classe tenha apenas uma instância e fornece um ponto global de acesso a essa instância.

**Quando Usar:** Quando você precisa garantir que apenas uma instância de uma classe seja criada durante o ciclo de vida da aplicação.

**Exemplo em Java:**

```java
public class Singleton {

    // Instância única da classe
    private static Singleton instance;

    // Construtor privado para evitar instanciamento direto
    private Singleton() {}

    // Método para retornar a única instância
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```

#### 4.1.2. Factory Method

O padrão **Factory Method** fornece uma interface para criar objetos, mas permite que as subclasses decidam qual classe instanciar. Isso ajuda a desacoplar a lógica de criação de objetos do código principal.

**Quando Usar:** Quando você tem uma classe que delega a responsabilidade de criação de objetos para subclasses.

**Exemplo em Java:**

```java
abstract class Produto {}

class ProdutoA extends Produto {}
class ProdutoB extends Produto {}

abstract class Fabrica {
    abstract Produto criarProduto();
}

class FabricaA extends Fabrica {
    Produto criarProduto() {
        return new ProdutoA();
    }
}

class FabricaB extends Fabrica {
    Produto criarProduto() {
        return new ProdutoB();
    }
}

```

### 4.2. Padrões Estruturais

#### 4.2.1. Adapter

O padrão **Adapter** permite que duas interfaces incompatíveis trabalhem juntas. Ele age como um adaptador que traduz a interface de uma classe para uma interface esperada por outra.

**Quando Usar:** Quando você precisa integrar classes ou sistemas que não foram projetados para funcionar juntos.

**Exemplo em Java:**

```java
// Interface esperada
interface Tomada {
    void ligarNaTomada();
}

// Classe existente
class TomadaBrasileira {
    public void conectar() {
        System.out.println("Conectado na tomada brasileira");
    }
}

// Adapter para conectar a tomada brasileira na interface esperada
class AdaptadorTomada implements Tomada {
    private TomadaBrasileira tomadaBrasileira;

    public AdaptadorTomada(TomadaBrasileira tomadaBrasileira) {
        this.tomadaBrasileira = tomadaBrasileira;
    }

    @Override
    public void ligarNaTomada() {
        tomadaBrasileira.conectar();
    }
}

```

#### 4.2.2. Decorator

O padrão **Decorator** permite que você adicione funcionalidades a um objeto de forma dinâmica, sem alterar sua estrutura original. Ele encapsula o objeto existente em um objeto decorador que adiciona a nova funcionalidade.

**Quando Usar:** Quando você quer adicionar responsabilidades a objetos de forma flexível, sem modificar a classe original.

**Exemplo em Java:**

```java
interface Cafe {
    String descricao();
    double preco();
}

class CafeSimples implements Cafe {
    public String descricao() {
        return "Café Simples";
    }

    public double preco() {
        return 2.00;
    }
}

class CafeComLeite implements Cafe {
    private Cafe cafe;

    public CafeComLeite(Cafe cafe) {
        this.cafe = cafe;
    }

    public String descricao() {
        return cafe.descricao() + " com Leite";
    }

    public double preco() {
        return cafe.preco() + 0.50;
    }
}

```

### 4.3. Padrões Comportamentais

#### 4.3.1. Strategy

O padrão **Strategy** permite que você defina uma família de algoritmos, encapsule cada um deles, e torne-os intercambiáveis. O padrão Strategy permite que o algoritmo varie independentemente dos clientes que o utilizam.

**Quando Usar:** Quando você tem múltiplos algoritmos para uma tarefa e quer trocar entre eles de maneira flexível.

**Exemplo em Java:**

```java
interface EstrategiaDePagamento {
    void pagar(double valor);
}

class PagamentoComCartao implements EstrategiaDePagamento {
    public void pagar(double valor) {
        System.out.println("Pagamento de " + valor + " feito com cartão.");
    }
}

class PagamentoComPaypal implements EstrategiaDePagamento {
    public void pagar(double valor) {
        System.out.println("Pagamento de " + valor + " feito com PayPal.");
    }
}

class CarrinhoDeCompras {
    private EstrategiaDePagamento estrategia;

    public CarrinhoDeCompras(EstrategiaDePagamento estrategia) {
        this.estrategia = estrategia;
    }

    public void finalizarCompra(double valor) {
        estrategia.pagar(valor);
    }
}

```

#### 4.3.2. Observer

O padrão **Observer** define uma dependência um-para-muitos entre objetos, de modo que quando um objeto muda de estado, todos os seus dependentes são notificados e atualizados automaticamente.

**Quando Usar:** Quando você tem um objeto que precisa notificar outros objetos sobre mudanças de estado.

**Exemplo em Java:**

```java
import java.util.ArrayList;
import java.util.List;

interface Observador {
    void atualizar(String mensagem);
}

class CanalNotificacoes {
    private List<Observador> observadores = new ArrayList<>();

    public void inscrever(Observador observador) {
        observadores.add(observador);
    }

    public void notificarTodos(String mensagem) {
        for (Observador observador : observadores) {
            observador.atualizar(mensagem);
        }
    }
}

class Usuario implements Observador {
    private String nome;

    public Usuario(String nome) {
        this.nome = nome;
    }

    @Override
    public void atualizar(String mensagem) {
        System.out.println(nome + " recebeu: " + mensagem);
    }
}

```

## 5. Quando Usar Design Patterns?

- **Soluções Comprovadas:** Se você encontrar um problema comum de design de software, há uma boa chance de que um padrão de projeto possa oferecer uma solução comprovada.
- **Melhoria na Manutenção:** Padrões de projeto ajudam a tornar o código mais legível e fácil de manter, especialmente em grandes equipes de desenvolvimento.
- **Código Flexível:** Muitos padrões ajudam a construir código flexível e extensível, facilitando futuras alterações e adições de funcionalidades.

## 6. Padrões Populares para Começar

Para desenvolvedores juniores, é uma boa ideia começar a se familiarizar com alguns dos padrões de projeto mais comuns, como:

1. **Singleton**
2. **Factory Method**
3. **Observer**
4. **Strategy**
5. **Decorator**

## 7. Conclusão

Os **Design Patterns** são ferramentas poderosas para escrever código mais robusto, manutenível e flexível. Embora possa parecer difícil no início, com o tempo e a prática, eles se tornam parte natural do seu fluxo de desenvolvimento, ajudando a resolver problemas comuns de forma eficiente.
