# ACID Properties

## 1. O que é ACID?

É um conjunto de propriedades que garantem que as transações em um banco de dados sejam processadas de maneira confiável. As propriedades ACID são extremamente importantes em sistemas de banco de dados que precisam assegurar consistência e integridade dos dados, como bancos de dados relacionais. Cada uma das letras no acrônimo **ACID** representa uma propriedade fundamental de uma transação:

- **A**: Atomicidade
- **C**: Consistência
- **I**: Isolamento
- **D**: Durabilidade

Esses princípios garantem que, mesmo em situações de falha (como uma queda de energia ou problemas de rede), os dados permanecem em um estado consistente.

## 2. Propriedades ACID

### 2.1. Atomicidade (Atomicity)

A atomicidade assegura que todas as operações dentro de uma transação sejam tratadas como uma **única unidade**. Ou todas as operações são completadas com sucesso, ou nenhuma delas é realizada. Se ocorrer algum erro no meio da transação, todas as operações realizadas até então são **desfeitas** (rollback).

- **Exemplo:** Suponha que você está transferindo dinheiro de uma conta bancária para outra. A transação envolve duas operações:
    1. Deduzir o valor da primeira conta.
    2. Adicionar o valor na segunda conta.

Se a primeira operação for bem-sucedida, mas a segunda falhar, a transação como um todo falhará, e o valor não será deduzido da primeira conta. Isso evita inconsistências, como o valor ser deduzido sem ser adicionado à outra conta.

### 2.2. Consistência (Consistency)

A propriedade de **consistência** assegura que uma transação leva o banco de dados de um estado **válido** para outro estado **válido**. Em outras palavras, as regras do banco de dados (como restrições de integridade, como chave primária, chave estrangeira ou restrições de tipo de dado) não devem ser violadas após a transação.

- **Exemplo:** Imagine que existe uma regra no banco de dados que proíbe saldos negativos em contas bancárias. Se uma transação tentar retirar mais dinheiro do que o saldo disponível, a transação será abortada para manter a consistência do banco de dados.

### 2.3. Isolamento (Isolation)

O **isolamento** garante que a execução simultânea de múltiplas transações não irá corromper os dados. Cada transação é executada como se fosse a única no sistema, sem interferir em outras transações. Isso significa que o resultado final de transações paralelas deve ser o mesmo como se elas fossem executadas uma após a outra (serialização).

- **Exemplo:** Suponha que dois usuários estão tentando transferir dinheiro da mesma conta ao mesmo tempo. O isolamento impede que as transações interfiram entre si, garantindo que o saldo da conta seja corretamente atualizado, sem resultar em perda de dinheiro ou duplicidade de operações.

### 2.4. Durabilidade (Durability)

A durabilidade assegura que, uma vez que uma transação é **confirmada** (committed), ela permanecerá persistente no banco de dados, mesmo em casos de falhas, como uma queda de energia ou um travamento do sistema. Isso é normalmente implementado utilizando técnicas de logging ou backups.

- **Exemplo:** Imagine que você finaliza a transação de transferência de dinheiro entre contas. Mesmo que haja uma falha de energia logo após a confirmação da transação, os dados da transação estarão seguros no banco de dados e disponíveis quando o sistema voltar.

## 3. Importância do ACID

As propriedades ACID são fundamentais para garantir a integridade dos dados e evitar corrupção em bancos de dados. Sem essas garantias, um banco de dados poderia facilmente cair em inconsistências, especialmente em sistemas de grande escala com múltiplas transações ocorrendo ao mesmo tempo.

## 4. Exemplos Práticos de ACID

### Exemplo com Atomicidade

```java

private Connection conn;

// Transação de banco: Transferência de fundos
public void transferirFundos(int deConta, int paraConta, double quantia) throws SQLException {
    try {
        conn.setAutoCommit(false);  // Inicia uma transação

        // Deduzir o valor da primeira conta
        PreparedStatement stmt1 = conn.prepareStatement("UPDATE contas SET saldo = saldo - ? WHERE id = ?");
        stmt1.setDouble(1, quantia);
        stmt1.setInt(2, deConta);
        stmt1.executeUpdate();

        // Adicionar o valor na segunda conta
        PreparedStatement stmt2 = conn.prepareStatement("UPDATE contas SET saldo = saldo + ? WHERE id = ?");
        stmt2.setDouble(1, quantia);
        stmt2.setInt(2, paraConta);
        stmt2.executeUpdate();

        // Commit da transação
        conn.commit();
    } catch (SQLException e) {
        // Rollback em caso de erro
        conn.rollback();
        throw e;
    }
}

```

Neste exemplo, se houver qualquer erro durante a transferência, como saldo insuficiente ou erro de conexão, a transação será revertida (rollback), garantindo a **atomicidade**.

### Exemplo com Consistência

Suponha que o banco de dados tenha uma restrição que proíbe saldos negativos. Se uma transação tentar deduzir um valor maior do que o saldo disponível, o banco de dados lançará um erro, mantendo o estado consistente.

```sql
ALTER TABLE contas
ADD CONSTRAINT saldo_positivo CHECK (saldo >= 0);

```

### Exemplo com Isolamento

Se dois clientes tentarem acessar e modificar os mesmos dados simultaneamente, o isolamento garante que as transações sejam tratadas adequadamente para evitar conflitos. Por exemplo, em um banco de dados com isolamento serializável, as transações seriam processadas como se fossem uma após a outra.

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

```

### Exemplo com Durabilidade

Bancos de dados garantem a durabilidade usando técnicas como logs de transação (write-ahead logs). Assim, mesmo que ocorra uma falha logo após a confirmação, o sistema pode restaurar o estado com segurança a partir do log.

## 5. Desafios de Implementação

Implementar as propriedades ACID pode ser desafiador em ambientes distribuídos, como bancos de dados NoSQL, onde a consistência eventual (em vez de consistência forte) é uma abordagem comum. No entanto, em bancos de dados relacionais tradicionais, como MySQL, PostgreSQL e Oracle, o ACID é amplamente suportado.

## 6. Quando Utilizar ACID?

As propriedades ACID são especialmente importantes em sistemas onde a **integridade dos dados** é crítica, como:

- Sistemas bancários.
- Sistemas de reservas (aéreas, hoteleiras).
- Sistemas de gerenciamento de estoque.

Em casos onde a **performance** é uma prioridade maior do que a consistência imediata, como em alguns bancos de dados NoSQL, uma abordagem diferente, como **BASE** (Basically Available, Soft State, Eventual Consistency), pode ser mais apropriada.

## 7. Considerações Finais

O modelo ACID é essencial para garantir que as transações em bancos de dados sejam confiáveis e que os dados permaneçam consistentes e duráveis, mesmo em casos de falhas. Um bom entendimento dessas propriedades ajuda desenvolvedores a criar sistemas mais seguros e confiáveis.