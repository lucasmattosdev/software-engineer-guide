# Idempotency Pattern

## 1. O que é o Idempotency Pattern?

É um padrão utilizado para garantir que uma operação produza o mesmo resultado, independentemente de quantas vezes ela seja executada. Ou seja, uma operação idempotente pode ser chamada múltiplas vezes sem efeitos colaterais adversos. Esse conceito é especialmente útil em sistemas distribuídos e comunicação entre serviços onde falhas podem levar à repetição de operações.

Em outras palavras, uma operação idempotente faz com que múltiplas execuções da mesma ação tenham o mesmo efeito que uma única execução, prevenindo a duplicação de resultados em caso de falhas ou reenvios de requisições.

## 2. Por que o Idempotency Pattern é Importante?

Em sistemas distribuídos e serviços baseados em HTTP (como APIs REST), é comum que ocorram falhas temporárias na comunicação, como timeouts ou erros de rede. Nessas situações, os clientes podem tentar enviar a mesma requisição várias vezes para garantir que a operação seja bem-sucedida.

Sem o Idempotency Pattern, múltiplas execuções da mesma operação podem levar a resultados inesperados ou incorretos. Por exemplo, sem idempotência, uma requisição de pagamento duplicada poderia resultar em uma cobrança dupla. Com idempotência, você garante que, mesmo que a mesma requisição seja enviada várias vezes, a ação será executada apenas uma vez, evitando problemas como:

- **Cobranças duplicadas**
- **Envio de emails repetidos**
- **Criação de registros duplicados no banco de dados**

## 3. Exemplos de Operações Idempotentes e Não Idempotentes

- **Operações Idempotentes:**
  - Leitura de dados (GET em APIs REST)
  - Atualização de dados com PUT
  - Exclusão de dados com DELETE (desde que bem implementada)
- **Operações Não Idempotentes:**
  - Criação de novos recursos (POST em APIs REST)
  - Processamento de pagamentos
  - Envio de emails ou notificações

### Exemplo 1: Operação Idempotente

Se você fizer uma requisição PUT para atualizar um recurso no banco de dados, independentemente de quantas vezes o fizer, o resultado será o mesmo: o recurso será atualizado com os mesmos dados.

### Exemplo 2: Operação Não Idempotente

Se você fizer múltiplas requisições POST para criar um novo pedido, poderá acabar com múltiplos pedidos no sistema. Sem idempotência, cada requisição POST criaria um novo registro.

## 4. Como o Idempotency Pattern Funciona?

O Idempotency Pattern é comumente implementado usando um **Idempotency Key** (Chave de Idempotência). O cliente gera uma chave única para cada operação que ele deseja garantir como idempotente. Essa chave é enviada junto com a requisição ao servidor, que verifica se a chave já foi usada anteriormente.

Se a chave for nova, o servidor processa a requisição normalmente e armazena o resultado associado àquela chave. Se a chave já foi usada, o servidor simplesmente retorna o resultado anterior, sem processar a requisição novamente.

### Passos Gerais

1. **Cliente envia uma requisição com uma Idempotency Key:**

    O cliente gera uma chave única para cada operação (pode ser um UUID ou hash) e a envia junto com a requisição ao servidor.

2. **Servidor verifica se a chave já foi usada:**

    O servidor verifica em um armazenamento (banco de dados, cache, etc.) se a chave já foi processada anteriormente.

3. **Processamento da requisição:**
    - Se a chave for nova, o servidor processa a operação e salva o resultado.
    - Se a chave já foi usada, o servidor retorna o resultado previamente salvo, ignorando a execução da operação novamente.
4. **Retorno da resposta:**

    O cliente recebe a resposta do servidor, seja o resultado de uma nova execução ou um resultado já processado anteriormente.

## 5. Exemplo de Implementação do Idempotency Pattern em Java

Aqui está um exemplo de como você poderia implementar o Idempotency Pattern em uma aplicação Java. Vamos usar uma API de criação de pedidos onde é necessário garantir que a mesma requisição não crie pedidos duplicados.

### 5.1. Passo 1: Criar a Tabela de Idempotency

Uma tabela no banco de dados para armazenar as chaves de idempotência e os resultados das requisições.

```sql
CREATE TABLE idempotency (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    idempotency_key VARCHAR(255) UNIQUE,
    result TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### 5.2. Passo 2: Implementação em Java

Aqui está um exemplo de código Java que utiliza o Idempotency Pattern.

```java
@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private IdempotencyRepository idempotencyRepository;

    @Transactional
    public Order createOrder(OrderRequest orderRequest, String idempotencyKey) {
        // Verificar se a chave de idempotência já foi usada
        Optional<IdempotencyRecord> existingRecord = idempotencyRepository.findByIdempotencyKey(idempotencyKey);

        if (existingRecord.isPresent()) {
            // Retornar o resultado anterior sem processar novamente
            return fromJson(existingRecord.get().getResult());
        }

        // Criar o novo pedido
        Order order = new Order();
        order.setProduct(orderRequest.getProduct());
        order.setQuantity(orderRequest.getQuantity());
        order.setTotalPrice(orderRequest.getPrice() * orderRequest.getQuantity());
        orderRepository.save(order);

        // Armazenar o resultado com a chave de idempotência
        IdempotencyRecord newRecord = new IdempotencyRecord(idempotencyKey, order.toJson());
        idempotencyRepository.save(newRecord);

        return order;
    }

```

## 6. Boas Práticas para Implementar o Idempotency Pattern

- **Use uma chave única para cada operação:** Gere um UUID ou use outra técnica confiável para garantir que a chave de idempotência seja única.
- **Valide a entrada do cliente:** Certifique-se de que o cliente está enviando a chave de idempotência corretamente em cada requisição.
- **Armazene o resultado completo da operação:** Armazene o resultado da operação idempotente para que você possa retorná-lo de forma confiável ao cliente em caso de reenvio da requisição.
- **Defina um tempo de expiração para as chaves:** Dependendo do caso de uso, você pode optar por expirar as chaves de idempotência após um período de tempo.

## 7. Desafios e Considerações

- **Persistência:** Você precisará de um mecanismo confiável para persistir as chaves de idempotência e os resultados das operações, seja em um banco de dados ou cache.
- **Escalabilidade:** O armazenamento das chaves de idempotência pode crescer rapidamente em sistemas de alta demanda. Considere estratégias de limpeza e expiração para evitar que os registros cresçam indefinidamente.
- **Operações complexas:** Para operações que envolvem múltiplas etapas (como criação de pedidos com múltiplos itens), a implementação pode se tornar mais complexa e exigir que você considere a idempotência em cada estágio da operação.

## 8. Conclusão

O **Idempotency Pattern** é uma técnica essencial em sistemas distribuídos para garantir que operações críticas não sejam duplicadas em casos de falhas ou reenvios de requisições. Ao implementar idempotência, você garante que sua aplicação seja mais confiável e resiliente, especialmente em cenários onde falhas de comunicação são comuns. Com a prática, você conseguirá aplicar esse padrão em diferentes contextos, como transações financeiras, APIs REST e sistemas de mensageria.
