# Rest Protocols

### Introdução ao REST

REST (Representational State Transfer) é um estilo de arquitetura amplamente utilizado para a criação de serviços web. Ele define um conjunto de restrições e princípios que orientam a construção de APIs (Application Programming Interfaces) que são simples, escaláveis e utilizam os padrões da web.

### Princípios Fundamentais do REST

1. **Cliente-Servidor**: Separação de responsabilidades. O cliente lida com a interface do usuário e o servidor gerencia os dados e a lógica.
2. **Stateless**: Cada requisição do cliente para o servidor deve conter todas as informações necessárias para entender e processar o pedido. O servidor não deve armazenar o estado da sessão.
3. **Cacheable**: Respostas podem marcadas como cacheáveis ou não para melhorar a eficiência da rede não exigindo novo processamento da mesma informação.
4. **Interface Uniforme**: Define uma interface uniforme entre componentes, facilitando a interação e a independência entre sistemas.
5. **Sistema em Camadas**: A arquitetura pode ser composta por várias camadas, como proxies e gateways, para melhorar a escalabilidade e segurança.

### Componentes Principais de uma API RESTful

- **Recursos (Resources)**: Entidades que a API manipula, identificadas por URLs.
- **Verbos HTTP**: Métodos para operar nos recursos. Os principais são:
    - `GET`: Recupera informações.
    - `POST`: Cria um novo recurso.
    - `PUT`: Atualiza um recurso existente.
    - `PATCH`: Atualiza parcialmente um recurso existente.
    - `DELETE`: Remove um recurso.
- **Request Body (opcional)**: Detalhamento da requisição para o servidor, geralmente em formato JSON.
- **Status Codes**: Códigos de resposta HTTP que indicam o resultado da operação, alguns exemplo:  `200 OK`, `404 Not Found`, `500 Internal Server Error`
- **Response Body (opcional)**: Detalhamento da resposta do servidor, geralmente em formato JSON.

### Boas Práticas de Design de API RESTful

1. **URI Design**:
    - Utilize substantivos e não verbos para os recursos: `/users` em vez de `/getUsers`.
    - Use barras (`/`) para indicar hierarquia: `/users/{userId}/orders`.
    - Use hífens (`-`) para legibilidade: `/user-orders` em vez de `/userOrders`.
2. **Utilize os Métodos HTTP Corretamente**:
    - `GET /users`: Recupera a lista de usuários.
    - `POST /users`: Cria um novo usuário.
    - `GET /users/{userId}`: Recupera um usuário específico.
    - `PUT /users/{userId}`: Atualiza um usuário específico.
    - `DELETE /users/{userId}`: Remove um usuário específico.
3. **HATEOAS (Hypermedia as the Engine of Application State)**:
    - Forneça links nas respostas para permitir que os clientes naveguem pela API.
4. **Versionamento**:
    - Versione sua API para manter compatibilidade: `/v1/users`.
5. **Autenticação e Autorização**:
    - Use padrões como OAuth para autenticação.
    - Proteja endpoints sensíveis.
6. **Documentação**:
    - Mantenha uma documentação clara e acessível, usando ferramentas como Swagger.

### Exemplo de Implementação

Aqui está um exemplo básico de uma API RESTful em Java com Springboot

### 1. Classe de Domínio (User)

Crie uma classe `User` simples para representar um usuário com `id` e `name`:

```java
@Value
public class User {
    private Long id;
    private String name;
}
```

### 3. Controlador REST (UserController)

Crie um controlador REST chamado `UserController` para manipular operações CRUD básicas para usuários:

```java
@RestController
@RequiredArgs
@RequestMapping("/users")
public class UserController {

		private final GetUserById getUserById;
		private final CreateUser createUser;
		private final UpdateUser updateUser;
		private final DeleteUser deleteUser;

    @GetMapping("/{userId}")
    public User getUserById(@PathVariable Long userId) {
        return getUserById.execute(userId)
                .orElseThrow(() -> new ResourceNotFoundException("User not found with id: " + userId));
    }

    @PostMapping
    public User createUser(@RequestBody UserRequest userRequest) {
        return createUser.execute(userRequest.toDomain())
    }

    @PutMapping("/{userId}")
    public User updateUser(@PathVariable Long userId, @RequestBody UserRequest userRequest) {
        return updateUser.execute(userId, userRequest.toDomain())
    }

    @DeleteMapping("/{userId}")
    public void deleteUser(@PathVariable Long userId) {
        deleteUser.execute(userId);
    }
}

```

### Ferramentas e Recursos

- **Postman**: Ferramenta para testar APIs.
- **Swagger**: Ferramentas para documentar e testar APIs.

### Conclusão

Seguir os princípios REST e boas práticas de design resulta em APIs que são fáceis de usar, manter e escalar. Este guia proporciona uma visão geral e prática sobre como construir e operar APIs RESTful de maneira eficiente.
