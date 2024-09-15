# Documentação de Projeto

## 1. O que é Documentação de Projeto?

É o conjunto de documentos que descreve o software, sua arquitetura, componentes e comportamento. Ela serve para orientar desenvolvedores, testers, gerentes de projeto e qualquer outro stakeholder envolvido no desenvolvimento e manutenção do sistema.

### 1.1. Por que a Documentação de Projeto é Importante?

- **Comunicação:** Facilita a troca de informações entre os membros da equipe.
- **Manutenção:** Ajuda a equipe de desenvolvimento a entender o sistema para futuras modificações ou correções.
- **Escalabilidade:** Orienta desenvolvedores a implementar novos recursos sem introduzir bugs.
- **Onboarding:** Acelera a curva de aprendizado de novos membros da equipe.

### 1.2. Tipos de Documentação

- **Documentação de Requisitos:** Descreve o que o sistema deve fazer (requisitos funcionais) e restrições (requisitos não funcionais).
- **Documentação Técnica:** Explica como o sistema foi projetado e implementado. Inclui diagramas de arquitetura, especificações de APIs, estruturas de banco de dados e lógica de negócio.
- **Documentação de Usuário:** Focada nos usuários finais, explicando como usar o sistema.
- **RFCs (Request for Comments):** Propostas formais para modificar ou adicionar novas funcionalidades ao projeto.

## 2. O que é RFC (Request for Comments)?

É um processo de documentação formal, amplamente utilizado na engenharia de software, para propor mudanças ou introduzir novas funcionalidades em um sistema ou projeto. O termo originalmente vem da IETF (Internet Engineering Task Force), onde os RFCs descrevem os padrões e protocolos da internet, mas hoje é comumente utilizado em projetos de software para melhorar a colaboração e revisão de propostas.

### 2.1. Estrutura de um RFC

Um RFC geralmente contém as seguintes seções:

- **Título:** Nome do RFC e o objetivo principal.
- **Contexto:** Detalha o problema que levou à criação do RFC.
- **Motivação:** Descreve o porque fazer as mudanças e que benefícios se planeja.
- **Definição:** Descrição detalhada da solução proposta.
- **Discussões e Comentários:** Sessão aberta para que os membros da equipe discutam, revisem e comentem sobre o RFC.

### 2.2. Benefícios do Uso de RFCs

- **Colaboração:** Permite que todos na equipe contribuam e discutam mudanças antes que sejam implementadas.
- **Rastreabilidade:** Fornece um histórico claro sobre por que determinadas mudanças foram feitas e quais decisões foram tomadas.
- **Clareza:** Reduz mal-entendidos, fornecendo uma descrição formal e padronizada das mudanças.

### 2.3. Exemplo de Uso

Imagine que você está desenvolvendo um sistema de autenticação e gostaria de adicionar autenticação via OAuth2. Antes de começar a implementação, você cria um RFC descrevendo as alterações necessárias, como impactarão o sistema atual, e os benefícios de usar OAuth2. O time revisa a proposta, faz sugestões e, após algumas iterações, a RFC é aprovada e a implementação começa com uma visão clara e acordada por todos.

## 3. C4 Model

É uma abordagem moderna para documentar a arquitetura de software, com um conjunto de diagramas de diferentes níveis que mostram a estrutura do sistema de maneira progressiva. O nome "C4" vem dos quatro níveis de diagrama: **Contexto, Contêiner, Componente e Código**.

### 3.1. Níveis do C4 Model

1. **Diagrama de Contexto:**
    - Mostra uma visão geral de alto nível do sistema, incluindo usuários e sistemas externos que interagem com ele.
    - **Objetivo:** Explicar o contexto em que o sistema está inserido.
    - **Exemplo:** Um sistema de e-commerce e suas interações com o banco e os usuários.
2. **Diagrama de Contêiner:**
    - Mostra os principais contêineres de software (como aplicações web, bancos de dados, microsserviços) e como eles se comunicam.
    - **Objetivo:** Explicar como o sistema está estruturado em termos de execução.
    - **Exemplo:** Frontend, Backend, Banco de Dados.
3. **Diagrama de Componente:**
    - Detalha os componentes individuais dentro de cada contêiner e como eles interagem entre si.
    - **Objetivo:** Explicar as principais partes do sistema dentro de um contêiner.
    - **Exemplo:** Um serviço de autenticação ou um controlador de pedidos no backend.
4. **Diagrama de Código:**
    - Foca em uma parte específica do sistema para mostrar como a lógica de negócio ou uma funcionalidade específica foi implementada.
    - **Objetivo:** Explicar a estrutura interna de um componente.
    - **Exemplo:** Um diagrama que mostra as classes e seus relacionamentos no código de autenticação.

### 3.2. Benefícios do C4 Model

- **Escalável:** Começa com uma visão geral e vai aumentando o nível de detalhe conforme necessário.
- **Simplicidade:** Evita diagramas excessivamente complexos, focando no que é essencial.
- **Clareza:** Torna fácil para desenvolvedores e arquitetos entenderem como o sistema é estruturado.

### 3.3. Exemplo de Uso do C4 Model

Um desenvolvedor pode começar com o diagrama de **Contexto** para entender o sistema como um todo, depois descer para o diagrama de **Contêiner** para ver a arquitetura e finalmente examinar o diagrama de **Componente** para detalhes sobre como os módulos se integram.

## 4. UML (Unified Modeling Language)

A **UML** é uma linguagem de modelagem visual padronizada, usada para especificar, visualizar, construir e documentar os artefatos de sistemas de software. Ela permite que desenvolvedores criem **diagramas** para representar a estrutura e o comportamento do sistema.

### 4.1. Principais Diagramas UML

1. **Diagrama de Classes:**
    - Representa a estrutura do sistema, mostrando as classes, seus atributos, métodos e relacionamentos.
    - **Objetivo:** Descrever a arquitetura orientada a objetos de um sistema.
    - **Exemplo:** Um diagrama que mostra classes como `Pedido`, `Produto`, `Cliente` e como elas se relacionam.
2. **Diagrama de Sequência:**
    - Mostra a interação entre diferentes objetos ao longo do tempo, focando na ordem das mensagens trocadas.
    - **Objetivo:** Descrever como os componentes interagem em um determinado fluxo de trabalho.
    - **Exemplo:** Um diagrama de sequência mostrando o processo de autenticação de um usuário.
3. **Diagrama de Caso de Uso:**
    - Foca no comportamento do sistema do ponto de vista do usuário, mostrando os casos de uso (funcionalidades) e os atores (usuários ou sistemas externos).
    - **Objetivo:** Identificar as principais funcionalidades que o sistema fornece e quem as utiliza.
    - **Exemplo:** Um diagrama de caso de uso para um sistema de reservas de voos, mostrando como os clientes podem procurar e reservar voos.
4. **Diagrama de Atividade:**
    - Representa o fluxo de atividades em um processo ou sistema.
    - **Objetivo:** Descrever processos dinâmicos e o fluxo de trabalho.
    - **Exemplo:** Um diagrama de atividade que mostra as etapas do processo de checkout em uma loja online.

### 4.2. Benefícios da UML

- **Padronizado:** É amplamente aceito e utilizado, tornando a comunicação entre desenvolvedores mais eficiente.
- **Versátil:** Pode ser usado para diversos tipos de sistemas (orientado a objetos, sistemas distribuídos, etc.).
- **Facilita a Documentação:** Ajuda a criar uma documentação técnica clara, que pode ser usada por toda a equipe.

### 4.3. Diferenças entre C4 Model e UML

- **C4 Model:** Focado na arquitetura de sistemas, com uma abordagem top-down. Começa com uma visão geral e depois desce para detalhes técnicos.
- **UML:** Mais voltado para modelagem de software orientado a objetos e fluxos de processo, com foco tanto na estrutura quanto no comportamento.

## 5. Ferramentas para Criar Diagramas

- **Excalidraw:** Ferramenta para modelagem e esboço em formato de desenho.
- **Draw.io:** Ferramenta gratuita online para criar diagramas UML e C4.
- **PlantUML:** Ferramenta baseada em código para criar diagramas UML.

## 6. Considerações Finais

A **documentação de projeto** é essencial para o sucesso a longo prazo de qualquer software, e usar modelos visuais como **C4 Model** e **UML** ajuda a clarificar a estrutura e o comportamento do sistema. O C4 Model é útil para visualizar a arquitetura de forma hierárquica, enquanto a UML oferece uma maneira detalhada de representar componentes e interações. Adotar o processo de **RFC** para propostas de mudança garante que as decisões sejam discutidas e registradas de maneira formal. Entender e aplicar essas práticas permitirá que o desenvolvedor júnior contribua de maneira mais eficaz em projetos de software.