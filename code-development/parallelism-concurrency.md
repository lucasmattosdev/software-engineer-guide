# Paralelismo e Concorrência

## 1. O que é Concorrência?

É a habilidade de um sistema para lidar com múltiplas tarefas ao mesmo tempo, mas não necessariamente executando-as simultaneamente. Na concorrência, diferentes tarefas progridem independentemente, intercalando suas execuções, permitindo que várias tarefas "compartilhem" o mesmo recurso (como a CPU), em vez de serem executadas uma após a outra.

Um sistema concorrente é projetado para permitir que várias operações sejam iniciadas, pausadas, retomadas ou finalizadas sem bloquear umas às outras. O foco da concorrência está na estruturação do programa para lidar com múltiplas atividades que interagem.

### Exemplo de Concorrência

Imagine que você está em uma cafeteria e faz um pedido. Enquanto o barista está preparando seu café, ele também está preparando os pedidos de outros clientes. Ele pode começar a fazer um café, interromper para atender outro cliente, e depois voltar ao seu pedido, fazendo com que várias tarefas sejam processadas "concorrentemente", mas nem sempre ao mesmo tempo.

### Características da Concorrência

- Tarefas podem ser executadas em **partes**, uma após a outra.
- Foco em gerenciar **várias atividades** em progresso, independentemente de estarem ocorrendo ao mesmo tempo.
- Requer o uso de técnicas como **threads**, **corrotinas** ou **processos leves**.

## 2. O que é Paralelismo?

**Paralelismo** ocorre quando várias tarefas são executadas simultaneamente, geralmente em processadores ou núcleos diferentes. Um sistema paralelo executa várias operações ao mesmo tempo, utilizando múltiplas unidades de processamento para acelerar a execução de tarefas.

O paralelismo requer hardware de suporte, como processadores com múltiplos núcleos ou máquinas com múltiplos CPUs. O foco está na execução simultânea de múltiplas tarefas para melhorar o desempenho e reduzir o tempo de execução.

### Exemplo de Paralelismo

Imagine que em vez de um único barista, há vários baristas na cafeteria. Cada barista está fazendo um café ao mesmo tempo, o que significa que vários pedidos estão sendo processados **simultaneamente**.

### Características do Paralelismo

- Tarefas são **executadas simultaneamente** em processadores diferentes.
- Requer **hardware multicore** ou distribuído.
- O objetivo é **acelerar** a execução, aproveitando a capacidade de processamento paralelo.

## 3. Diferença entre Concorrência e Paralelismo

Embora muitas vezes usados como sinônimos, concorrência e paralelismo não são a mesma coisa.

- **Concorrência** trata de **múltiplas tarefas** sendo realizadas de forma intercalada (elas podem não acontecer ao mesmo tempo).
- **Paralelismo** trata de **múltiplas tarefas** sendo realizadas **ao mesmo tempo** em diferentes núcleos de CPU.

Em outras palavras, a concorrência é sobre estruturar o programa para lidar com várias tarefas "ao mesmo tempo", enquanto o paralelismo é sobre **executar** essas tarefas ao mesmo tempo.

| **Concorrência** | **Paralelismo** |
| --- | --- |
| Foco em **gerenciar** várias tarefas em progresso. | Foco em **executar** várias tarefas simultaneamente. |
| Pode ocorrer em uma única CPU. | Requer múltiplos núcleos ou CPUs. |
| Usa técnicas como **threads**, **corrotinas**, ou **event loops**. | Usa múltiplos núcleos de CPU para execução simultânea. |

## 4. Concorrência em Java

O Java possui suporte nativo para **concorrência** por meio da classe `Thread` e da API de concorrência no pacote `java.util.concurrent`. Aqui está um exemplo simples de concorrência em Java usando threads.

### Exemplo: Concorrência com Threads

```java
java
Copy code
public class ConcurrentExample {

    public static void main(String[] args) {
        // Cria duas threads
        Thread thread1 = new Thread(new Task("Tarefa 1"));
        Thread thread2 = new Thread(new Task("Tarefa 2"));

        // Inicia as threads
        thread1.start();
        thread2.start();
    }
}

class Task implements Runnable {
    private String name;

    public Task(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + " está em execução. Iteração: " + i);
            try {
                // Simula trabalho com uma pausa
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

### Explicação

- O código cria duas threads (`Tarefa 1` e `Tarefa 2`).
- As duas tarefas são executadas de maneira **concorrente**. A CPU alterna entre as threads, permitindo que ambas avancem no processamento.

## 5. Paralelismo em Java

Para aproveitar o **paralelismo**, você pode utilizar a **API de streams paralelos** do Java ou bibliotecas como o **ForkJoinPool**. O paralelismo é particularmente útil em sistemas com múltiplos núcleos de CPU.

### Exemplo: Paralelismo com Streams Paralelos

```java
java
Copy code
import java.util.Arrays;
import java.util.List;

public class ParallelExample {

    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Processar a lista de forma paralela
        numbers.parallelStream().forEach(number -> {
            System.out.println("Processando número: " + number + " - Thread: " + Thread.currentThread().getName());
        });
    }
}

```

### Explicação

- Neste exemplo, o Java utiliza vários núcleos de CPU para processar os números da lista em paralelo.
- A API `parallelStream()` permite que a lista seja dividida entre diferentes threads, aproveitando o **paralelismo** disponível no hardware.

## 6. Quando Utilizar Concorrência e Paralelismo?

### Use Concorrência Quando

- Você precisa estruturar o sistema para lidar com várias tarefas ao mesmo tempo, mas sem a necessidade de processamento simultâneo.
- O objetivo é melhorar a **responsividade** do sistema, como em aplicações web onde múltiplas requisições são tratadas simultaneamente.

### Use Paralelismo Quando

- Você tem múltiplos núcleos de CPU disponíveis e deseja **acelerar** o processamento de tarefas computacionalmente intensivas, como algoritmos de processamento de grandes volumes de dados.
- O objetivo é melhorar a **performance**, especialmente em sistemas que exigem alta capacidade de processamento.

## 7. Desafios de Concorrência e Paralelismo

### Concorrência

- **Condições de Corrida (Race Conditions):** Ocorrências onde múltiplas threads tentam acessar e modificar um recurso compartilhado ao mesmo tempo, levando a inconsistências.
- **Deadlocks:** Situação onde duas ou mais threads ficam presas esperando por um recurso que nunca será liberado.

### Paralelismo

- **Overhead de Comunicação:** Em ambientes paralelos, o custo de coordenar múltiplos processadores pode às vezes ser maior que o ganho de desempenho.
- **Escalabilidade:** Nem todas as tarefas podem ser paralelizadas de forma eficiente. Algumas operações dependem de outras e precisam ser realizadas de forma sequencial.

### Ambos

- **Incompatibilidade com frameworkds:** Alguns frameworks utilizam a thread principal (ThreadLocal) para persistir informações e usar durante o ciclo de vida da chamada (como conexões de banco de dados, usuário que requisitou, detalhes da chamada principal) e caso os dados não sejam clonados para a segunda thread criada, ao necessitar destes dados o processo em execução pela thread secundária não os terá causando errors ao processar.

## 8. Conclusão

**Concorrência** e **paralelismo** são conceitos fundamentais para o desenvolvimento de sistemas escaláveis e eficientes. Enquanto a concorrência foca em gerenciar várias tarefas que podem ou não ocorrer ao mesmo tempo, o paralelismo aproveita múltiplas CPUs para executar tarefas simultaneamente. Entender quando e como usar cada um é crucial para criar sistemas modernos que sejam tanto rápidos quanto responsivos.
