# Code Review

## 1. O que é Code Review?

É o processo de verificar o código escrito por outros desenvolvedores para garantir que ele esteja correto, eficiente, legível e atenda aos padrões de qualidade da equipe ou da empresa. Geralmente, isso é feito antes que o código seja integrado ao código-fonte principal (merge).

Durante o code review, outros desenvolvedores examinam o código em busca de possíveis bugs, melhorias de performance, violações de estilo, e discutem como o código poderia ser melhorado.

## 2. Por que Code Review é Importante?

Code review traz vários benefícios para a qualidade do código e para o time de desenvolvimento:

- **Detecção de Erros:** Revisores podem encontrar bugs ou problemas lógicos que o autor do código pode não ter percebido.
- **Melhoria de Qualidade:** Revisões de código ajudam a garantir que o código atenda aos padrões de qualidade da equipe e siga boas práticas de desenvolvimento.
- **Compartilhamento de Conhecimento:** Revisar o código de colegas ajuda a todos a aprenderem novos padrões, técnicas e funcionalidades. Isso promove um ambiente de aprendizado contínuo.
- **Melhoria da Legibilidade:** O código é lido mais vezes do que escrito. Um bom code review melhora a legibilidade, tornando-o mais fácil de manter e expandir.

## 3. Tipos de Code Review

Existem diferentes maneiras de conduzir code reviews. Aqui estão algumas das mais comuns:

### 3.1. Revisão Assíncrona

Este é o tipo mais comum de code review, onde o autor do código cria um pull request (PR) ou merge request (MR) e os revisores comentam o código de forma assíncrona. Os revisores analisam o código, sugerem mudanças, e o autor atualiza o código até que esteja pronto para ser aprovado.

- **Ferramentas:** GitHub Pull Requests, GitLab Merge Requests, Bitbucket Pull Requests.

### 3.2. Revisão Síncrona (Pair Programming)

Nesse tipo de revisão, dois desenvolvedores trabalham juntos, lado a lado, para escrever e revisar o código em tempo real. Esse formato pode ser eficaz para problemas complexos ou para compartilhar conhecimento rapidamente.

- **Ferramentas:** VS Code Live Share, Zoom, Google Meet.

### 3.3. Revisão em Reuniões (Over-the-Shoulder)

Nesta abordagem, um desenvolvedor revisa o código de outro pessoalmente ou em uma chamada de vídeo, com o autor do código presente. Essa técnica é útil para discussões mais detalhadas, mas menos escalável do que a revisão assíncrona.

## 4. Como Conduzir um Code Review

### 4.1. Passos para um Code Review Eficiente

1. **Leia o Contexto:** Antes de revisar o código, entenda o que a mudança está tentando resolver. Leia a descrição do pull request e as issues associadas.
2. **Verifique a Correção do Código:** Garanta que o código realmente resolva o problema proposto ou adicione a funcionalidade desejada.
3. **Legibilidade:** O código é fácil de entender? Está bem comentado onde necessário? Variáveis e funções têm nomes significativos?
4. **Boas Práticas e Padrões:** O código segue as boas práticas de desenvolvimento (ex: SOLID, Clean Code)? Está alinhado com o estilo e padrões definidos pela equipe?
5. **Performance:** Existe alguma melhoria de performance que poderia ser feita? O código é eficiente?
6. **Segurança:** Existem possíveis vulnerabilidades de segurança? O código lida com entradas de usuário de maneira segura?
7. **Testes:** O código está bem testado? Existem testes automatizados cobrindo os cenários relevantes? Os testes existentes ainda passam?

### 4.2. Comunicação durante o Code Review

- **Seja Respeitoso:** Lembre-se de que o objetivo do code review é melhorar o código, e não criticar o autor. Ofereça sugestões construtivas e seja sempre educado.
- **Seja Objetivo:** Aponte problemas específicos e sugira soluções concretas. Evite generalizações vagas como "este código está confuso" sem explicar o porquê.
- **Pergunte quando estiver em dúvida:** Se algo não estiver claro, pergunte ao invés de assumir algo errado.

**Exemplo de Feedback Construtivo:**

- **Bom:** "Eu acho que a função `calcularDesconto` poderia ser mais legível se dividirmos em funções menores. Talvez possamos extrair a lógica de validação para uma função separada. O que você acha?"
- **Ruim:** "Esse código está confuso. Refatore isso."

### 4.3. Recebendo Feedback

Quando você é o autor do código, é importante receber o feedback de forma positiva. Code reviews são uma oportunidade para aprender e melhorar seu código.

- **Não leve para o lado pessoal:** O feedback é sobre o código, não sobre você.
- **Aceite Sugestões:** Esteja aberto a mudanças e melhorias, mesmo que isso signifique retrabalhar parte do código.
- **Discuta quando necessário:** Se você discordar de um comentário, converse sobre isso de forma respeitosa. Muitas vezes, uma boa discussão pode levar a uma solução ainda melhor.

## 5. Boas Práticas de Code Review

### 5.1. Para Revisores

- **Foque nas Mudanças Relevantes:** Não se prenda a detalhes triviais (como formatação) se ferramentas automatizadas já cuidam disso. Concentre-se em problemas de lógica, bugs, e legibilidade.
- **Seja Rápido:** Tente revisar o código em um tempo razoável para não bloquear o trabalho do autor. Idealmente, revisões devem ocorrer dentro de 24 horas após o envio do pull request.
- **Tamanho das Mudanças:** Incentive pull requests pequenos. É mais fácil revisar uma mudança pequena com poucas linhas de código do que uma grande alteração com milhares de linhas.

### 5.2. Para Autores

- **Divida Pull Requests Grandes:** Tente enviar pull requests menores e mais frequentes, em vez de uma única grande mudança. Isso facilita a revisão e o merge.
- **Escreva uma Boa Descrição:** Inclua uma descrição clara no pull request explicando o que foi feito e por quê. Isso ajuda o revisor a entender o contexto rapidamente.
- **Garanta que os Testes Passam:** Antes de enviar seu código para revisão, garanta que todos os testes estejam passando e que você tenha adicionado testes para novas funcionalidades.
- **Sempre melhore o código:** Busque trazer evolução continua para o projeto, melhorando a parte na qual você teve dificuldades para entender, ou que esteja fora dos padrões acordado com o time.

## 6. Ferramentas de Code Review

Existem várias ferramentas que facilitam o processo de code review, permitindo comentários linha por linha, discussões em torno do código e integração contínua com ferramentas de CI/CD. Abaixo estão algumas populares:

- **GitHub:** Ferramenta integrada para revisar pull requests, com suporte a comentários linha a linha e integração com pipelines de CI.
- **GitLab:** Oferece funcionalidades similares ao GitHub, com suporte adicional a revisões em merge requests e pipelines de CI/CD.
- **Bitbucket:** Plataforma com suporte a pull requests, comentários e integração com CI/CD (Bitbucket Pipelines).

## 7. Exemplo Prático de Code Review

Aqui está um exemplo prático de um código que pode ser revisado. Suponha que você recebeu o seguinte código para revisão:

```java
public class CalculadoraDePreco {
    public double calcularPrecoFinal(double precoInicial, double desconto) {
        if (desconto > 0) {
            return precoInicial - (precoInicial * desconto / 100);
        } else {
            return precoInicial;
        }
    }
}
```

**Feedback Sugerido:**

- **Correção:** O código funciona corretamente, mas falta validação para garantir que `desconto` seja um valor entre 0 e 100.
- **Legibilidade:** A lógica poderia ser extraída para uma função separada para maior clareza. Além disso, o nome da função poderia ser mais descritivo, como `aplicar_desconto`.
- **Testes:** Certifique-se de que há testes cobrindo os cenários de desconto zero, descontos positivos, e valores inválidos.

```java
public class CalculadoraDePreco {
    public double aplicarDesconto(double precoInicial, double desconto) {
        validarDesconto(desconto);
        return precoInicial - (precoInicial * desconto / 100);
    }

    private void validarDesconto(double desconto) {
        if (desconto < 0 || desconto > 100) {
            throw new IllegalArgumentException("Desconto deve ser entre 0 e 100.");
        }
    }
}
```

## 8. Conclusão

Code review é uma prática essencial no desenvolvimento de software para garantir que o código seja de alta qualidade, seguro e mantenível. Ele também promove o compartilhamento de conhecimento e o crescimento contínuo de toda a equipe. Ao seguir as melhores práticas e manter uma comunicação clara e respeitosa, o processo de code review se torna uma parte valiosa do ciclo de desenvolvimento.
