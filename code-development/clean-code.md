# Clean Code

## 1. O que é Clean Code?

É um conceito introduzido por Robert C. Martin (conhecido como Uncle Bob) em seu livro *Clean Code: A Handbook of Agile Software Craftsmanship*. Escrever código limpo significa escrever código que seja fácil de entender, fácil de modificar e que siga boas práticas de desenvolvimento.

A ideia principal é que o código será lido e mantido muitas vezes ao longo de seu ciclo de vida. Portanto, um código limpo deve ser simples, direto e organizado, facilitando o trabalho de outros desenvolvedores (e de você mesmo no futuro).

## 2. Por que Clean Code é Importante?

- **Facilidade de Manutenção:** Código limpo é mais fácil de entender, o que facilita a correção de bugs e a adição de novas funcionalidades.
- **Colaboração:** Quando várias pessoas trabalham no mesmo projeto, um código limpo e organizado facilita a colaboração.
- **Redução de Erros:** Código confuso e desorganizado tende a introduzir mais bugs. Um código limpo reduz a probabilidade de erros.
- **Produtividade a Longo Prazo:** Embora escrever código limpo possa levar um pouco mais de tempo inicialmente, isso compensa em produtividade no longo prazo, já que o código será mais fácil de modificar e expandir.

## 3. Princípios de Clean Code

### 3.1. Nomes Significativos

O uso de **nomes significativos** para variáveis, funções, classes e métodos é essencial para tornar o código mais legível e autoexplicativo. Nomes devem refletir a intenção de maneira clara, sem a necessidade de comentários adicionais.

**Exemplo:**

```java
// Ruim
int d; // tempo decorrido em dias

// Melhor
int diasDecorridos;

```

- **Dica:** Evite abreviações desnecessárias e use nomes que comuniquem o propósito do código.

### 3.2. Funções Pequenas e Simples

Funções devem ser **pequenas e realizar uma única tarefa**. Se uma função está fazendo mais de uma coisa, considere refatorá-la em funções menores.

**Exemplo:**

```java
// Ruim
public void processarPagamento(Pedido pedido) {
    // Validações do pedido

    // Cálculo do total

    // Lógica de autorização de pagamento

    // Envio de confirmação
}

// Melhor
public void processarPagamento(Pedido pedido) {
    validarPedido(pedido);
    calcularTotal(pedido);
    autorizarPagamento(pedido);
    enviarConfirmacao(pedido);
}

private void validarPedido(Pedido pedido) {
    // Validações do pedido
}

private void calcularTotal(Pedido pedido) {
    // Cálculo do total
}

private void autorizarPagamento(Pedido pedido) {
    // Lógica de autorização de pagamento
}

private void enviarConfirmacao(Pedido pedido) {
    // Envio de confirmação
}

```

- **Dica:** Mantenha as funções focadas em uma única responsabilidade, conforme o Princípio da Responsabilidade Única (SRP) do SOLID.

### 3.3. Comentários Úteis e Objetivos

Embora o objetivo seja que o código se explique por si só, **comentários** podem ser úteis em situações onde o código precisa de contexto adicional. No entanto, comentários não devem ser usados para explicar o óbvio ou compensar um código mal escrito.

**Exemplo:**

```java
// Ruim
// Seta o valor da variável 'idade' para 30
int idade = 30;

// Melhor
// A idade padrão de novos usuários é 30
int idade = 30;

```

- **Dica:** Prefira escrever código claro em vez de adicionar comentários excessivos. Quando precisar comentar, foque no *porquê* algo está sendo feito, não *o que* está sendo feito.

### 3.4. Evite Código Duplicado

Código duplicado é um dos maiores inimigos de um código limpo. Sempre que possível, reutilize código em vez de copiá-lo e colá-lo.

**Exemplo:**

```java
// Ruim
public void processaSemNotificacao() {
    // logica processamento 
}

public void processaComNotificacao() {
    // logica processamento 

    notify();
}

// Melhor
public void processaSemNotificacao() {
    // logica processamento 
}

public void processaComNotificacao() {
    processaSemNotificacao(); 

    notify();
}

```

- **Dica:** Identifique padrões de repetição no seu código e extraia esses padrões para funções ou classes reutilizáveis.

### 3.5. Tratamento de Erros Claro

Erros devem ser tratados de maneira clara e explícita. Evite capturar exceções genéricas e forneça mensagens de erro úteis que ajudem a diagnosticar o problema.

**Exemplo:**

```java
// Ruim
try {
    // código
} catch (Exception e) {
    // silêncio ou mensagem vaga
}

// Melhor
try {
    // código
} catch (SQLException e) {
    log.error("Erro ao acessar o banco de dados", e);
    throw new BancoDeDadosException("Não foi possível acessar o banco de dados", e);
}

```

- **Dica:** Escreva mensagens de erro que sejam informativas e não capture exceções genéricas, pois isso pode esconder problemas inesperados.

### 3.6. Código Pequeno e Focado

O código deve ser **focado e direto**. Evite complexidade desnecessária, código morto (código que nunca é executado) ou código comentado.

**Exemplo:**

```java
// Ruim
public class OrderProcessor {

    public double processOrder(Order order) throws Exception {
        // Valida o pedido
        if (!order.isValid()) {
            throw new Exception("Pedido inválido");
        }

        // Calcula o preço total
        double totalPrice = 0;
        for (Item item : order.getItems()) {
            totalPrice += item.getPrice() * item.getQuantity();
        }

        // Aplica desconto
        if (order.getCoupon() != null) {
            totalPrice -= order.getCoupon().getDiscount();
        }

        // Atualiza o estoque
        for (Item item : order.getItems()) {
            item.setStock(item.getStock() - item.getQuantity());
        }

        // Envia e-mail de confirmação
        EmailService.sendConfirmationEmail(order.getCustomer().getEmail());

        return totalPrice;
    }
}

// Melhor
public class OrderProcessor {

    public double processOrder(Order order) throws Exception {
        validateOrder(order);
        double totalPrice = calculateTotalPrice(order);
        updateStock(order);
        EmailService.sendConfirmationEmail(order.getCustomer().getEmail());
        return totalPrice;
    }

    private void validateOrder(Order order) throws Exception {
        if (!order.isValid()) {
            throw new Exception("Pedido inválido");
        }
    }

    private double calculateTotalPrice(Order order) {
        double totalPrice = 0;
        for (Item item : order.getItems()) {
            totalPrice += item.getPrice() * item.getQuantity();
        }
        if (order.getCoupon() != null) {
            totalPrice -= order.getCoupon().getDiscount();
        }
        return totalPrice;
    }

    private void updateStock(Order order) {
        for (Item item : order.getItems()) {
            item.setStock(item.getStock() - item.getQuantity());
        }
    }
}
```

- **Dica:** Sempre prefira simplicidade sobre complexidade. Evite sobrecarregar suas funções com lógica desnecessária.

## 4. Boas Práticas de Clean Code

- **Leia e Revise o Código:** Revise o código constantemente para identificar áreas que podem ser melhoradas.
- **Prefira Confiabilidade ao Aceleramento:** Priorize escrever código claro e confiável ao invés de código rápido e sujo.
- **Refatore Regularmente:** Mantenha o código limpo, revisando e refatorando sempre que necessário.
- **Testes Automatizados:** Código limpo é código testável. Escreva testes automatizados para garantir que seu código continua funcionando conforme esperado após mudanças.

## 5. Exemplo de Código Limpo em Java

Aqui está um exemplo de código limpo que incorpora alguns dos princípios discutidos:

```java
public class Calculadora {

    public int somar(int a, int b) {
        return a + b;
    }

    public int subtrair(int a, int b) {
        return a - b;
    }

    public int multiplicar(int a, int b) {
        return a * b;
    }

    public double dividir(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Divisão por zero não é permitida");
        }
        return (double) a / b;
    }
}

```

- **Nomes Significativos:** Os métodos têm nomes que indicam claramente o que fazem.
- **Funções Simples:** Cada método tem uma única responsabilidade.
- **Tratamento de Erros Claro:** O método `dividir` trata explicitamente o erro de divisão por zero.

## 6. Conclusão

**Clean Code** é uma prática essencial para qualquer desenvolvedor que deseja escrever código que seja legível, fácil de manter e de alta qualidade. Ao seguir os princípios de Clean Code, você não apenas melhora a qualidade do seu próprio trabalho, mas também facilita a colaboração com outros desenvolvedores e a manutenção do código a longo prazo.

Escrever código limpo exige disciplina e prática, mas os benefícios em termos de manutenibilidade, clareza e redução de bugs fazem valer o esforço. Comece aplicando esses princípios no seu dia a dia e verá uma melhoria significativa na qualidade do seu código.
