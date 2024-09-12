
# Outbox Pattern

## 1. O que é o Outbox Pattern?

É um padrão arquitetural usado para garantir a consistência de dados em cenários onde há persistencia de dados em base de dados e a chamadas a APIs ou notificação através de serviço de mensageria. Usado principalmente em sistemas distribuídos para resolver problemas de **consistência eventual**.

Em uma arquitetura de microserviços, por exemplo, é comum que uma operação precise:

- **Persistir dados** no banco de dados.
- **Notificar outro serviço externo** via API ou mensageria (como Kafka, RabbitMQ, etc.).

O Outbox Pattern garante que ambas as operações aconteçam de maneira **atômica** (ou seja, ambas acontecem ou nenhuma acontece), evitando problemas como a perda de mensagens ou inconsistências em casos de falha. Para isso, normalmente é utilizado um dos princípios ACID de banco de dados relacional ou simulado a atômicidade de forma manual em bancos não relacionais.

## 2. Problema que o Outbox Pattern Resolve

Em sistemas distribuídos, uma operação de atualização de banco de dados e a notificação de outro serviço externo geralmente precisam ser **consistentes**. Por exemplo, imagine o seguinte cenário:

1. O serviço A persiste uma ordem de compra no banco de dados.
2. O serviço A envia uma mensagem para um sistema de mensageria (Kafka ou RabbitMQ) notificando outro serviço B de que uma nova ordem foi criada.

Se o banco de dados é atualizado com sucesso, mas a mensagem não é enviada (por causa de uma falha na rede, por exemplo), o serviço B nunca será notificado da nova ordem. Isso cria uma **inconsistência** no sistema.

O Outbox Pattern resolve esse problema garantindo que ambas as operações (persistência no banco e notificação ao serviço externo) sejam tratadas de forma confiável e atômica.

## 3. Como o Outbox Pattern Funciona?

O padrão Outbox funciona gravando as mudanças de estado (eventos) em uma tabela separada do banco de dados chamada **Outbox Table** ao mesmo tempo que as mudanças principais são feitas. Um processo separado (ou thread) então lê as mensagens dessa tabela e as envia para o sistema externo ou de mensageria.

### 3.1. Passos

1. **Transação principal:**
    - O serviço persiste a operação (por exemplo, criar um pedido) e insere um registro na **Outbox Table** dentro da mesma transação no banco de dados.
2. **Processamento da Outbox:**
    - Um processo (ou worker) dedicado lê os registros da Outbox Table e envia a mensagem correspondente para o serviço externo (como um sistema de mensageria ou API externa).
3. **Confirmação de envio:**
    - Após o envio bem-sucedido, o processo remove (ou marca como enviada) a mensagem da Outbox Table.

## 4. Vantagens do Outbox Pattern

- **Consistência garantida:** O padrão garante que, se o dado foi gravado no banco de dados, a mensagem será enviada ao serviço externo, mesmo que haja falhas intermediárias.
- **Escalabilidade:** Ao separar a operação de gravação da comunicação com serviços externos, o padrão facilita a escalabilidade do sistema, já que o processo de envio pode ser realizado de forma assíncrona.
- **Resiliência a falhas:** Em caso de falha no serviço externo ou na mensageria, as mensagens permanecem na Outbox Table e serão reprocessadas quando o sistema estiver disponível novamente.

## 5. Desvantagens do Outbox Pattern

- **Complexidade adicional:** O Outbox Pattern adiciona complexidade ao sistema, já que exige a criação de um mecanismo para ler e processar as mensagens da Outbox Table.
- **Latência:** O envio das mensagens para o serviço externo ocorre de maneira assíncrona, o que pode adicionar um pequeno atraso na comunicação.
- **Necessidade de gestão de falhas:** O processo de envio da Outbox deve lidar adequadamente com falhas, como mensagens que não puderam ser enviadas ou processadas corretamente.

## 6. Exemplo de Implementação em Java

Aqui está um exemplo de como implementar o Outbox Pattern em Java, usando uma aplicação que precisa salvar uma ordem de compra no banco de dados e enviar uma notificação para outro serviço via Kafka.

### 6.1. Passo 1: Criar a Tabela Outbox

Primeiro, você cria a tabela **Outbox** no banco de dados.

```sql
CREATE TABLE outbox (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    event_type VARCHAR(255),
    payload TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```

### 6.2. Passo 2: Inserir na Outbox dentro de uma Transação

No código Java, você insere o evento na **Outbox Table** no mesmo contexto de transação da operação principal (salvar a ordem de compra).

```java
@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private OutboxRepository outboxRepository;

    @Transactional
    public void createOrder(Order order) {
        // Salvar a ordem no banco de dados
        orderRepository.save(order);

        // Criar um evento de outbox
        OutboxEvent event = new OutboxEvent("order-created", order.toJson());
        outboxRepository.save(event);  // Inserir na tabela outbox
    }
}

```

### 6.3. Passo 3: Processar a Outbox e Enviar as Mensagens

Um processo em segundo plano lê a tabela **Outbox** e envia as mensagens para o sistema de mensageria (neste caso, Kafka).

```java
@Service
public class OutboxProcessor {

    @Autowired
    private OutboxRepository outboxRepository;

    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    @Scheduled(fixedRate = 5000)  // Executar a cada 5 segundos
    public void processOutbox() {
        List<OutboxEvent> events = outboxRepository.findByProcessedFalse();

        for (OutboxEvent event : events) {
            try {
                // Enviar a mensagem para Kafka
                kafkaTemplate.send("orders", event.getPayload());

                // Retira o processo da fila
                outboxRepository.delete(event);
            } catch (Exception e) {
                // Logar e tentar novamente na próxima execução
                System.err.println("Falha ao processar evento: " + e.getMessage());
            }
        }
    }
}
```

## 7. Conclusão

O **Outbox Pattern** é uma poderosa ferramenta para garantir a consistência de dados entre um banco de dados local e sistemas externos, como serviços de mensageria ou APIs. Ele é particularmente útil em arquiteturas de microserviços, onde as operações distribuídas precisam ser confiáveis. Apesar de adicionar certa complexidade ao sistema, a sua capacidade de garantir a consistência e resiliência a falhas o torna uma solução viável para muitos cenários.
