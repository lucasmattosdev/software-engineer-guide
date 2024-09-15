# API Design

## 1. O que é uma API?

Uma **API** (*Application Programming Interface*) é uma interface que permite que diferentes sistemas de software se comuniquem entre si. Ela define um conjunto de regras para que aplicações possam enviar e receber dados, sem precisar entender os detalhes internos umas das outras.

APIs podem ser usadas para:

- Permitir que diferentes serviços de uma aplicação se comuniquem (por exemplo, em uma arquitetura de microservices).
- Expor funcionalidades de uma aplicação para outras aplicações (por exemplo, APIs públicas de redes sociais).
- Conectar aplicativos móveis ou frontend web a backends.

## 2. O que é API Design?

**API Design** se refere ao processo de projetar a interface de uma API de maneira que ela seja fácil de usar, eficiente e consistente. Um bom design de API garante que desenvolvedores consigam integrar seus sistemas de maneira eficiente, sem erros ou ambiguidades, ao mesmo tempo em que respeita padrões e boas práticas.

## 3. Tipos de API

Existem diversos tipos de APIs, dependendo do estilo de comunicação utilizado:

- **REST (Representational State Transfer):** Um estilo de arquitetura que usa HTTP e URLs para realizar operações em recursos. É amplamente usado devido à sua simplicidade.
- **SOAP (Simple Object Access Protocol):** Um protocolo mais formal e estruturado, geralmente usado em sistemas que requerem padrões estritos de segurança e transações.
- **GraphQL:** Uma linguagem de consulta para APIs que permite aos clientes solicitarem exatamente os dados de que precisam, e nada mais.
- **gRPC:** Um framework de RPC (Remote Procedure Call) que permite comunicação rápida e eficiente entre serviços.

## 4. Boas Práticas de API Design

Um bom design de API deve ser **consistente**, **intuitivo** e **fácil de entender**. Aqui estão algumas boas práticas para garantir que sua API seja bem projetada:

### 4.1. Use URLs Claras e Descritivas

URLs devem ser simples e descrever claramente o recurso que está sendo acessado. Elas geralmente seguem o conceito de **recurso** e **sub-recurso**.

- Use **substantivos** e não **verbos** para definir os endpoints.
- A URL deve refletir a hierarquia dos recursos.

**Exemplo:**

- Para listar todos os usuários: `GET /users`
- Para obter detalhes de um usuário específico: `GET /users/{userId}`
- Para listar todos os pedidos de um usuário: `GET /users/{userId}/orders`

### 4.2. Utilize os Métodos HTTP Corretamente

A API RESTful depende dos verbos HTTP para definir ações a serem tomadas sobre os recursos. Aqui estão os métodos HTTP mais comuns e seu uso recomendado:

- **GET:** Usado para recuperar informações de um recurso (sem alterar o estado).
- **POST:** Usado para criar um novo recurso.
- **PUT/PATCH:** Usado para atualizar um recurso existente.
    - **PUT**: Atualiza todo o recurso.
    - **PATCH**: Atualiza parcialmente um recurso.
- **DELETE:** Usado para remover um recurso.

**Exemplo:**

- Para criar um novo usuário: `POST /users`
- Para atualizar as informações de um usuário: `PUT /users/{userId}`
- Para excluir um usuário: `DELETE /users/{userId}`

### 4.3. Use Códigos de Status HTTP Apropriadamente

Sempre retorne o **código de status HTTP** correto nas respostas, para que os clientes da API saibam o resultado da requisição. Alguns dos códigos mais comuns são:

- **200 OK:** A requisição foi bem-sucedida.
- **201 Created:** Um novo recurso foi criado.
- **400 Bad Request:** O cliente enviou uma requisição inválida.
- **401 Unauthorized:** O cliente não tem permissão para acessar o recurso.
- **404 Not Found:** O recurso não foi encontrado.
- **500 Internal Server Error:** Houve um erro no servidor.

### 4.4. Padrão de Nomes Consistente

Manter consistência no design dos endpoints é crucial. Use convenções de nomenclatura simples e consistentes.

- Use **plural** para recursos: `/users`, `/products`.
- Evite caracteres especiais e maiúsculas nas URLs: `/users/{userId}` é melhor que `/Users/{UserId}`.

### 4.5. Suporte a Versionamento de API

Com o tempo, sua API pode evoluir, e é importante garantir que mudanças não quebrem implementações antigas dos clientes. Uma maneira comum de fazer isso é através do versionamento de API.

**Exemplo de versionamento na URL:**

- `GET /v1/users`
- `GET /v2/users`

Ou usando **headers**:

```
GET /users
Accept: application/vnd.myapi.v1+json
```

### 4.7. Validação e Tratamento de Erros

Sempre valide as entradas recebidas em suas APIs e forneça mensagens de erro claras e úteis. Isso facilita o uso da API por parte dos desenvolvedores.

**Exemplo de erro ao enviar dados inválidos:**

```json
{
  "status": 400,
  "code": "invalid-mail",
  "message": "Invalid email format"
}
```

### 4.8. Documentação Completa e Acessível

Uma boa API sempre vem acompanhada de uma documentação clara e acessível. A documentação deve cobrir:

- Como os endpoints funcionam.
- Que parâmetros devem ser fornecidos (obrigatórios e opcionais).
- Exemplos de requisições e respostas.
- Códigos de erro e mensagens.

Ferramentas como **Swagger** (OpenAPI) e **Postman** podem ajudar a gerar documentação interativa e detalhada.

## 5. Exemplos de API Design

### 5.1. Exemplo de API RESTful Simples

Vamos considerar um exemplo de API para gerenciar **usuários** e seus **pedidos**.

- **GET /users**: Retorna uma lista de usuários.
- **GET /users/{userId}**: Retorna os detalhes de um usuário específico.
- **POST /users**: Cria um novo usuário.
- **PUT /users/{userId}**: Atualiza as informações de um usuário.
- **DELETE /users/{userId}**: Remove um usuário.
- **GET /users/{userId}/orders**: Retorna os pedidos de um usuário.
- **POST /users/{userId}/orders**: Cria um novo pedido para um usuário.

### 5.2. Exemplo de API com Versionamento

Se você precisar fazer mudanças em sua API, como adicionar novos campos ou alterar o comportamento de alguns endpoints, é importante versionar a API para não quebrar os clientes existentes.

- **Versão 1:** `GET /v1/users`
- **Versão 2:** `GET /v2/users` (pode incluir novos campos ou comportamentos diferentes).

## 6. Ferramentas Úteis para API Design

- **Swagger/OpenAPI:** Ferramenta popular para documentação interativa de APIs.
- **Postman:** Ferramenta para testar APIs e gerar documentação.
- **Insomnia:** Ferramenta semelhante ao Postman, útil para testar APIs.
- **RAML (RESTful API Modeling Language):** Linguagem para descrever APIs RESTful.

## 7. Considerações Finais

Projetar uma **API** requer mais do que apenas expor endpoints. Ela deve ser **consistente**, **bem documentada** e seguir padrões estabelecidos para facilitar a adoção por outros desenvolvedores. Uma API bem projetada é intuitiva, fácil de usar e evolui de forma sustentável com o tempo. Ao seguir boas práticas de **API Design**, você pode garantir que sua API seja robusta, escalável e fácil de manter.