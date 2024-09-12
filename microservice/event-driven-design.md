# Event-Driven Design

## 1. O que é Event-Driven Design?

É um padrão arquitetural em que os componentes de um sistema comunicam-se entre si através de eventos utilizando [queue service](/microservice/queue-service). Ao invés de interações diretas entre módulos, os eventos são emitidos para notificar a ocorrência de algo importante no sistema, e outros componentes podem reagir a esses eventos de maneira assíncrona.

No **EDD**, um **evento** é qualquer mudança significativa no estado do sistema, como a criação de um pedido, a conclusão de uma transação ou a modificação de um registro de usuário. A arquitetura é altamente **desacoplada**, onde os emissores de eventos não precisam saber quem está escutando ou reagindo a esses eventos.

### Exemplo Simples

Imagine um sistema de e-commerce onde, ao criar um pedido, o sistema deve:

- Atualizar o estoque de produtos.
- Enviar um e-mail de confirmação ao cliente.
- Processar o pagamento.

Em um design tradicional, essas operações seriam realizadas de forma sequencial, com dependências entre os módulos. No **Event-Driven Design**, o sistema apenas emite um **evento de pedido criado**, e outros serviços ou módulos (como o de estoque, e-mail e pagamento) reagem ao evento de forma assíncrona, sem que o serviço de criação de pedido saiba quem está escutando.

## 2. Componentes do Event-Driven Design

No **Event-Driven Design**, há três componentes principais:

1. **Emissor de Eventos (Event Producer):**
    - Este é o componente que detecta ou provoca a ocorrência de um evento e o publica. Ele não precisa saber como o evento será processado ou quem irá consumi-lo. Exemplo: Serviço de criação de pedidos emite um evento "Pedido Criado".
2. **Consumidor de Eventos (Event Consumer):**
    - Um ou mais componentes que "ouvem" os eventos e reagem a eles de alguma forma. Um consumidor pode realizar uma ação como atualizar um banco de dados, chamar uma API ou emitir outro evento em resposta ao evento recebido. Exemplo: Um serviço de estoque que reduz a quantidade disponível de um produto quando recebe o evento de "Pedido Criado".
3. **Barramento de Eventos (Event Broker):**
    - Um middleware que gerencia a distribuição dos eventos. Ele recebe eventos do produtor e os entrega aos consumidores registrados. Exemplos de sistemas de barramento incluem RabbitMQ, Kafka ou AWS SNS/SQS.

## 3. Tipos de Eventos

Os eventos podem ser classificados em dois tipos principais:

- **Eventos de Estado:** Notificam sobre uma mudança no estado de um recurso. Por exemplo, "Pedido Criado", "Pagamento Confirmado", "Produto Atualizado".
- **Eventos de Comando:** Representam uma ação que precisa ser realizada, como "Processar Pagamento" ou "Enviar Notificação de E-mail".

## 4. Benefícios do Event-Driven Design

- **Desacoplamento:** Os produtores de eventos não precisam saber nada sobre os consumidores, e vice-versa. Isso torna o sistema mais flexível e fácil de manter.
- **Escalabilidade:** Como os eventos são processados de forma assíncrona, os componentes podem ser escalados independentemente, permitindo um melhor balanceamento de carga.
- **Resiliência:** Caso um consumidor falhe, o evento pode ser reprocessado mais tarde, garantindo que o sistema seja mais tolerante a falhas.
- **Flexibilidade:** Novos consumidores podem ser adicionados sem alterar o código dos produtores, facilitando a evolução do sistema.

## 5. Desafios do Event-Driven Design

- **Complexidade Adicional:** Como o fluxo de trabalho é distribuído e assíncrono, pode ser mais difícil rastrear e debugar problemas.
- **Entrega Garantida:** Nem todos os sistemas de eventos garantem entrega. Certifique-se de que o barramento de eventos ou o middleware escolhido oferece opções de entrega confiável.
- **Consistência Eventual:** Em sistemas altamente distribuídos, os consumidores podem processar eventos em tempos diferentes, levando a situações de **consistência eventual**, onde o estado de todos os serviços pode levar algum tempo para se sincronizar.

## 6. Exemplos de Implementação

Vamos a um exemplo prático de **Event-Driven Design** em uma aplicação Java, utilizando Kafka como sistema de mensageria.

### 6.1. Passo 1: Produzir um Evento

```java
@Service
public class OrderService {

    @Autowired
    private KafkaTemplate<String, OrderEvent> kafkaTemplate;

    public void createOrder(Order order) {
        // Lógica de criação do pedido
        saveOrder(order);

        // Criar evento de pedido criado
        OrderEvent event = new OrderEvent(order.getId(), order.getTotalPrice());

        // Publicar evento no Kafka
        kafkaTemplate.send("order-events", event);
    }
}

```

### 6.2. Passo 2: Consumir o Evento

```java
@Service
public class StockService {

    @KafkaListener(topics = "order-events", groupId = "stock-group")
    public void handleOrderEvent(OrderEvent event) {
        // Lógica para atualizar o estoque baseado no pedido
        updateStock(event.getOrderId());
    }
}

```

### 6.3. Modelo do Evento

```java
@Value
public class OrderEvent {
    String orderId;
    double totalPrice;

    public OrderEvent(String orderId, double totalPrice) {
        this.orderId = orderId;
        this.totalPrice = totalPrice;
    }
}

```

## 7. Quando Utilizar o Event-Driven Design?

O **Event-Driven Design** é particularmente útil em situações como:

- **Sistemas de microserviços:** Onde serviços diferentes precisam interagir e reagir a eventos de forma desacoplada.
- **Sistemas de alta disponibilidade:** Onde a resiliência é essencial e falhas em um componente não devem impactar o restante do sistema.
- **Cenários de alto volume de dados:** Quando há necessidade de processar grandes volumes de eventos, como em sistemas de logs ou monitoramento.

## 8. Exemplos do Mundo Real

- **Sistemas de pagamento:** Onde um evento de "pagamento processado" pode desencadear atualizações no banco de dados, envio de e-mails, ou ativação de uma conta.
- **Plataformas de e-commerce:** Onde um evento de "pedido realizado" pode iniciar a redução do estoque, a geração de faturas e a preparação para envio.
- **Redes sociais:** Onde um evento de "postagem criada" pode notificar seguidores, atualizar feeds e iniciar análises de engajamento.

## 9. Conclusão

O **Event-Driven Design** permite que sistemas complexos, distribuídos e escaláveis sejam desenvolvidos de forma flexível e resiliente. Ao utilizar eventos para comunicar mudanças de estado entre os serviços, é possível construir sistemas que reagem a mudanças de maneira eficiente, sem a necessidade de forte acoplamento entre os componentes.

Embora esse padrão ofereça muitos benefícios, ele também pode introduzir novos desafios, como a necessidade de gerenciar consistência eventual e lidar com falhas de comunicação entre os componentes. Com a prática e o uso adequado de ferramentas como Kafka, RabbitMQ ou outras plataformas de mensageria, você pode dominar esse padrão e aplicá-lo em diversos cenários.
