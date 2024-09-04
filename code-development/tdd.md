# Test-Driven Development (TDD)

## 1. O que é Test-Driven Development (TDD)?

É uma metodologia de desenvolvimento de software em que os testes automatizados são escritos antes do código de produção. O conceito principal do TDD é que, para cada funcionalidade que você deseja implementar, você começa escrevendo um teste que descreve o comportamento desejado. Somente depois de criar o teste, você escreve o código para fazê-lo passar.

O TDD segue o ciclo **Red-Green-Refactor**, que será descrito detalhadamente abaixo.

## 2. Por que Usar TDD?

- **Maior Confiança no Código:** Com testes automatizados escritos antes do código, você pode ter mais confiança de que o sistema está funcionando conforme o esperado.
- **Desenvolvimento Focado:** O TDD ajuda a focar no que o código realmente precisa fazer, levando ao desenvolvimento de código mais simples e direto.
- **Refatoração Segura:** Como você já tem testes cobrindo o comportamento do sistema, pode refatorar o código com segurança, sabendo que os testes vão garantir que o comportamento não foi quebrado.
- **Documentação Automatizada:** Os testes funcionam como uma documentação viva do sistema, descrevendo como as funcionalidades devem se comportar.

## 3. O Ciclo Red-Green-Refactor

O TDD segue um ciclo contínuo conhecido como **Red-Green-Refactor**:

1. **Red (Teste Falha):** Escreva um teste que descreva o comportamento desejado para uma funcionalidade. O teste inicialmente falhará, pois a funcionalidade ainda não foi implementada.
2. **Green (Teste Passa):** Escreva o código mínimo necessário para que o teste passe. O foco aqui é simplesmente fazer o teste passar, sem se preocupar com otimizações ou organização do código.
3. **Refactor (Refatoração):** Refatore o código para melhorar sua estrutura, mantendo todos os testes passando. Isso inclui remover duplicação, melhorar a legibilidade e otimizar o código.

Esse ciclo se repete para cada nova funcionalidade ou melhoria no sistema.

## 4. Exemplo Prático de TDD em Java

Vamos usar um exemplo simples para ilustrar o fluxo de TDD. Suponha que queremos criar uma classe `Calculadora` com um método `somar` que soma dois números.

### 4.1. Red (Escrever o Teste Primeiro)

Começamos escrevendo o teste que descreve o comportamento esperado do método `somar`.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculadoraTest {

    @Test
    public void testSomarDoisNumeros() {
        Calculadora calculadora = new Calculadora();
        int resultado = calculadora.somar(2, 3);
        assertEquals(5, resultado);
    }
}

```

- Neste ponto, o teste falha porque a classe `Calculadora` e o método `somar` ainda não existem.

### 4.2. Green (Escrever o Código Mínimo)

Agora, criamos a classe `Calculadora` e implementamos o método `somar` de maneira mais simples possível para passar o teste.

```java
public class Calculadora {

    public int somar(int a, int b) {
        return a + b;
    }
}

```

- Após essa implementação, o teste deve passar, pois o comportamento esperado foi atendido.

### 4.3. Refactor (Refatoração do Código)

Neste exemplo simples, o código já está claro e conciso, então não há necessidade de refatoração. No entanto, em sistemas mais complexos, essa etapa seria usada para melhorar a estrutura do código sem alterar o comportamento testado.

## 5. Boas Práticas no TDD

- **Escreva Testes Pequenos e Isolados:** Cada teste deve cobrir uma pequena parte da funcionalidade, focando em um comportamento específico.
- **Refatore Sempre que Necessário:** Após garantir que o código está funcionando com os testes, refatore o código para melhorar a qualidade sem medo de quebrar a funcionalidade.
- **Mantenha o Ciclo Curto:** O ciclo Red-Green-Refactor deve ser curto. Evite escrever muito código antes de rodar os testes.
- **Cobertura de Código Não é Tudo:** O TDD não é apenas sobre ter testes que cubram o código, mas sim sobre garantir que o código faz exatamente o que deveria fazer.

## 6. Quando Usar TDD?

- **Desenvolvimento de Novas Funcionalidades:** Ao implementar novas funcionalidades, o TDD ajuda a garantir que o comportamento desejado está correto desde o início.
- **Correção de Bugs:** Escrever um teste que reproduza o bug antes de corrigi-lo ajuda a garantir que o problema foi resolvido e que ele não voltará no futuro.
- **Refatoração de Código Legado:** Antes de refatorar código legado, escreva testes para cobrir o comportamento existente e garantir que a refatoração não introduza regressões.

## 7. Benefícios do TDD

- **Menos Bugs em Produção:** Ao testar o código antes de implementá-lo, é menos provável que bugs escapem para produção.
- **Código Mais Simples:** O TDD leva à escrita de código mais simples e direto, já que você se foca apenas em fazer o teste passar.
- **Documentação do Comportamento:** Os testes escritos no TDD funcionam como documentação do comportamento do sistema, facilitando a compreensão do código por outros desenvolvedores.

## 8. Desafios do TDD

- **Curva de Aprendizado:** No início, o TDD pode parecer contra-intuitivo, especialmente para desenvolvedores que estão acostumados a escrever código antes dos testes.
- **Tempo Adicional:** Escrever testes antes do código pode parecer que toma mais tempo, mas isso é compensado pela redução de bugs e pela facilidade de manutenção no longo prazo.
- **Cobertura de Casos de Bordas:** Garantir que todos os casos extremos e cenários possíveis sejam cobertos pelos testes pode ser um desafio.

## 9. Conclusão

O **Test-Driven Development (TDD)** é uma prática poderosa que leva a um código mais confiável, simples e fácil de manter. Ao adotar TDD, você desenvolve com mais segurança, reduzindo a probabilidade de bugs e melhorando a qualidade do código. Embora possa exigir uma mudança de mentalidade e prática, os benefícios em termos de manutenibilidade e confiança no sistema valem o esforço.
