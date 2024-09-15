# Circuit Breaker

## 1. O que é o Circuit Breaker Pattern?

É um padrão de design usado para melhorar a **resiliência** e **confiabilidade** de sistemas distribuídos, especialmente em microservices. Seu principal objetivo é evitar chamadas repetidas para serviços que estão com problemas ou indisponíveis, protegendo o sistema de sobrecargas e falhas em cascata.

Assim como em um disjuntor elétrico, o **Circuit Breaker** "desarma" quando detecta problemas, impedindo novas tentativas de acesso ao serviço até que ele se recupere.

## 2. Problema que o Circuit Breaker Resolve

Em sistemas distribuídos, um serviço pode depender de outro para funcionar. Se um serviço falhar ou estiver indisponível (por exemplo, devido a sobrecarga ou falha na rede), continuar enviando requisições para ele pode:

- Sobrecarregar ainda mais o serviço;
- Congestionar a rede;
- Degradar o desempenho geral do sistema;
- Propagar falhas para outros serviços que dependem dele.

O **Circuit Breaker** evita esses problemas interrompendo temporariamente as requisições a um serviço que está falhando, dando tempo para ele se recuperar.

## 3. Estados do Circuit Breaker

O Circuit Breaker tem três estados principais:

1. **Closed (Fechado):**
    - Nesse estado, o Circuit Breaker permite que todas as requisições passem normalmente para o serviço.
    - O sistema continua monitorando as requisições. Se muitas delas falharem (acima de um limite predefinido), o Circuit Breaker **"desarma"** e entra no estado **Open**.
2. **Open (Aberto):**
    - Quando o número de falhas ultrapassa o limite definido, o Circuit Breaker "abre", bloqueando todas as requisições para o serviço.
    - Durante esse estado, todas as requisições falham imediatamente, sem nem mesmo serem enviadas ao serviço falho.
    - Após um tempo determinado (um **timeout**), o Circuit Breaker entra no estado **Half-Open**.
3. **Half-Open (Meio-Aberto):**
    - Nesse estado, algumas requisições são enviadas para verificar se o serviço voltou a funcionar.
    - Se as requisições de teste forem bem-sucedidas, o Circuit Breaker volta para o estado **Closed**.
    - Se as requisições de teste falharem, o Circuit Breaker retorna ao estado **Open**.

## 4. Fluxo de Funcionamento

- Quando o serviço está funcionando corretamente, o Circuit Breaker permanece **Closed** e as requisições fluem normalmente.
- Se uma série de falhas ocorre (por exemplo, 5 falhas consecutivas), o Circuit Breaker entra no estado **Open**, interrompendo as requisições para o serviço problemático.
- Após um tempo de espera, o Circuit Breaker tenta novamente (estado **Half-Open**) enviar algumas requisições. Se essas funcionarem, ele volta ao estado **Closed**. Se falharem, o Circuit Breaker volta a **Open**.

## 5. Quando Usar o Circuit Breaker

O **Circuit Breaker** deve ser usado em cenários onde:

- Seu sistema depende de serviços externos que podem falhar ou ficar indisponíveis.
- Há risco de **falhas em cascata** quando um serviço crítico fica indisponível e outras partes do sistema dependem dele.
- Você deseja evitar que requisições contínuas para um serviço com problemas degradem ainda mais o desempenho geral do sistema.

Exemplos típicos:

- Sistemas de pagamento que dependem de gateways de pagamento externos.
- Aplicações que se comunicam com APIs de terceiros.
- Microservices que dependem de serviços em outras partes do sistema.

## 6. Exemplo de Implementação do Circuit Breaker

Aqui está um exemplo simples de como o **Circuit Breaker** pode ser implementado em Java usando a biblioteca **Resilience4j**.

### 6.1. Dependência no Maven

```xml
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-circuitbreaker</artifactId>
    <version>1.7.0</version>
</dependency>

```

### 6.2. Exemplo de Código

```java
import io.github.resilience4j.circuitbreaker.CircuitBreaker;
import io.github.resilience4j.circuitbreaker.CircuitBreakerConfig;
import io.github.resilience4j.circuitbreaker.CircuitBreakerRegistry;

import java.time.Duration;

public class CircuitBreakerExample {
    public static void main(String[] args) {
        // Configurando o Circuit Breaker
        CircuitBreakerConfig config = CircuitBreakerConfig.custom()
            .failureRateThreshold(50) // Limite de falhas: 50%
            .waitDurationInOpenState(Duration.ofSeconds(10)) // Tempo de espera no estado Open
            .slidingWindowSize(5) // Tamanho da janela deslizante
            .build();

        // Criando o Circuit Breaker
        CircuitBreakerRegistry registry = CircuitBreakerRegistry.of(config);
        CircuitBreaker circuitBreaker = registry.circuitBreaker("myCircuitBreaker");

        // Executando uma chamada protegida pelo Circuit Breaker
        String result = circuitBreaker.executeSupplier(() -> {
            // Simulando uma chamada para um serviço externo
            return callExternalService();
        });

        System.out.println(result);
    }

    public static String callExternalService() {
        // Simula uma falha no serviço
        if (Math.random() > 0.7) {
            return "Resposta do Serviço Externo";
        } else {
            throw new RuntimeException("Falha no Serviço Externo");
        }
    }
}

```

Nesse exemplo:

- O Circuit Breaker está configurado para abrir (estado **Open**) se mais de 50% das requisições falharem.
- Ele permanece no estado **Open** por 10 segundos antes de tentar novamente (estado **Half-Open**).
- O método `executeSupplier` executa a chamada para o serviço externo, protegendo-o pelo Circuit Breaker.

## 7. Benefícios do Circuit Breaker

- **Resiliência:** Protege seu sistema contra falhas propagadas de serviços externos.
- **Eficiência:** Evita o envio de requisições inúteis para serviços que estão temporariamente fora do ar.
- **Recuperação Gradual:** Permite uma recuperação controlada, com tentativas graduais de reconectar o serviço.

## 8. Ferramentas e Bibliotecas

- **Resilience4j:** Biblioteca popular para implementar Circuit Breaker e outros padrões de resiliência em Java.
- **Netflix Hystrix:** Uma biblioteca de código aberto que também implementa o Circuit Breaker (embora esteja em fase de manutenção).
- **Spring Cloud Circuit Breaker:** Abstração que suporta várias bibliotecas, incluindo Resilience4j e Hystrix.

## 9. Considerações Finais

O **Circuit Breaker Pattern** é fundamental para sistemas distribuídos e microservices que dependem de serviços externos. Ele melhora a **resiliência**, evitando que falhas de um serviço impactem todo o sistema. Implementá-lo corretamente pode garantir que sua aplicação se recupere mais rapidamente de falhas, mantendo a experiência do usuário estável.
