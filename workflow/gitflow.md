# Gitflow

## 1. O que é Gitflow?

É uma estratégia de ramificação (branching) para projetos que utilizam um controle de versão como o Git. Ele define um conjunto de regras e práticas para a criação, nomeação e fusão de ramificações, ajudando a organizar o desenvolvimento de software em diferentes estágios (desenvolvimento, testes, produção).

Gitflow é amplamente utilizado por equipes de desenvolvimento que trabalham com ciclos de lançamento, oferecendo uma maneira eficiente de gerenciar **feature branches**, **releases** e **correções de bugs** em um repositório Git.

## 2. Benefícios do Gitflow

- **Organização:** Estrutura clara para desenvolvimento de novas funcionalidades e manutenção de releases.
- **Paralelismo:** Permite que desenvolvedores trabalhem em diferentes funcionalidades simultaneamente sem interferir no código estável.
- **Controle de Versão:** Mantém as versões de produção separadas do código em desenvolvimento.
- **Integração Contínua:** Facilita a integração contínua (CI/CD), onde o código é constantemente testado e integrado.

## 3. Componentes do Gitflow

O Gitflow organiza o repositório em diferentes tipos de ramificações (branches), cada uma com uma finalidade específica:

### 3.1. Branch `main`

A branch **main** (ou `master`, em alguns fluxos) contém o código **pronto para produção**. Todas as versões lançadas estão aqui, e o código na `main` sempre deve estar estável.

- **Objetivo:** Contém a versão de produção do software.
- **Uso:** Nenhuma feature ou fix (a não ser que seja um hotfix) é feita diretamente na `main`. Novos recursos são integrados via merge de outras branches.

### 3.2. Branch `develop`

A branch **develop** contém o código mais recente em desenvolvimento. Todas as novas funcionalidades são eventualmente mescladas aqui. É a principal branch de trabalho para desenvolvimento ativo.

- **Objetivo:** Integração de novas funcionalidades e preparação para o próximo release.
- **Uso:** Novas funcionalidades e correções são mescladas aqui a partir de branches de feature ou bugfix.

### 3.3. Branch `feature`

As branches de **feature** são criadas para desenvolver **novas funcionalidades**. Elas são derivadas da branch `develop` e, quando concluídas, são mescladas de volta nela.

- **Nomeação:** Usualmente nomeadas como `feature/nome-da-feature`.
- **Objetivo:** Desenvolvimento de uma nova funcionalidade específica.
- **Uso:** Quando uma nova funcionalidade precisa ser desenvolvida, um desenvolvedor cria uma branch de feature a partir da `develop`.

### 3.4. Branch `fix`

As branches de **fix** são criadas para aplicar alguma **correção ou melhoria**. Elas são derivadas da branch `develop` e, quando concluídas, são mescladas de volta nela.

- **Nomeação:** Usualmente nomeadas como `fix/nome-da-feature`.
- **Objetivo:** Desenvolvimento de correção ou melhoria não crítica.
- **Uso:** Utilizada para correções ou melhorias que não possuem possuem urgência como as hotfix.


### 3.5. Branch `release`

As branches de **release** são criadas quando o desenvolvimento em `develop` está pronto para ser lançado. Elas servem para testar e corrigir bugs antes de serem mescladas na branch `main`.

- **Nomeação:** Usualmente nomeadas como `release/x.x.x`, onde `x.x.x` é o número da versão.
- **Objetivo:** Preparar o código para um lançamento oficial.
- **Uso:** Bugs encontrados na fase de release são corrigidos diretamente nesta branch.

### 3.6. Branch `hotfix`

As branches de **hotfix** são usadas para corrigir **bugs críticos** em produção. Elas são derivadas diretamente da branch `main` e, após corrigidas, são mescladas tanto na `main` quanto na `develop`.

- **Nomeação:** Usualmente nomeadas como `hotfix/nome-do-bug`.
- **Objetivo:** Resolver um problema crítico rapidamente.
- **Uso:** Se um bug crítico é encontrado em produção, um hotfix é criado diretamente a partir da `main`.

## 4. Fluxo de Trabalho com Gitflow

Aqui está uma visão geral do fluxo de trabalho típico ao usar Gitflow:

1. **Desenvolvimento de uma nova funcionalidade:**
    - Crie uma branch `feature` a partir da `develop`.
    - Trabalhe na nova funcionalidade gerando outras branches para criar PRs pequenos para esta branch de feature.
    - Quando a feature estiver pronta, mescle-a de volta à branch `develop`.
2. **Preparação para o lançamento:**
    - Quando todas as funcionalidades estiverem prontas para o próximo lançamento, crie uma branch `release` a partir da `develop`.
    - Resolva bugs encontrados durante os testes da release e faça commits na branch `release`.
    - Quando a release estiver pronta para ser lançada, mescle a branch `release` na `main` e depois gere backport da `main` para a `develop`.
3. **Correção de bugs críticos:**
    - Se um bug crítico for encontrado em produção, crie uma branch `hotfix` a partir da `main`.
    - Corrija o bug e faça commits na branch `hotfix`.
    - Quando a correção estiver completa, mescle a branch `hotfix` na `main` e depois gere backport da `main` para a `develop`.

## 5. Exemplo Completo de Uso do Gitflow

Vamos simular um ciclo completo de desenvolvimento usando Gitflow.

### Passo 1: Desenvolvimento de uma Nova Funcionalidade

```bash
# Vá para a branch develop
git checkout develop

# Atualize sua branch local
git pull

# Crie uma nova branch de feature
git checkout -b feature/nova-funcionalidade
git push

# Crie uma nova branch de trabalho pequena a ser enviado para a feature
git checkout -b feature/nova-funcionalidade/caso-de-uso

# Trabalhe na nova funcionalidade e faça commits
git add .
git commit -m "feat: caso de uso para fazer x virar y"
git push
```

Ao finalizar um pedaço da feature, crie um PR fazendo o merge para a feature.

Ao finalizar toda a feature, se ela for entregue na proxima release, crie um PR fazendo o merge para a develop, se não, a mantenha na branch release até que se planeje a entrega dela na próxima release

### Passo 2: Preparação de um Release

```bash
# Crie uma branch de release a partir da develop
git checkout develop
git checkout -b release/1.0.0

# Resolva bugs caso seja encontrados durante os testes e faça commits
git add .
git commit -m "Corrige bugs da release 1.0.0"
git push
```

Ao finalizar os testes, crie um PR fazendo o merge para a main

Ao realizar o merge, crie um PR fazendo o merge da main para a develop

### Passo 3: Correção de Bug Crítico com Hotfix

```bash
# Crie uma branch de hotfix a partir da main
git checkout main
git pull
git checkout -b hotfix/falha-ao-criar-usuario

# Corrija o bug e faça commits
git add .
git commit -m "fix: tratamento de campos obrigatorios na criacao de usuario"
git push
```

Ao finalizar os testes, crie um PR fazendo o merge para a main

Ao realizar o merge, crie um PR fazendo o merge da main para a develop

Ps: lembre-se sempre de excluir a branch após os PRs.

## 6. Quando Utilizar Gitflow?

Gitflow é recomendado para equipes que trabalham com **ciclos de lançamento definidos** e precisam de um fluxo de trabalho organizado para gerenciar desenvolvimento contínuo, lançamentos e correções de bugs críticos. Ele é ideal para:

- Projetos com vários desenvolvedores.
- Equipes que seguem uma metodologia **ágil** ou **Scrum**.
- Projetos que exigem **entregas frequentes** e precisam gerenciar múltiplos ambientes (desenvolvimento, teste, produção).

## 7. Considerações Finais

É uma estratégia poderosa e flexível para gerenciar o ciclo de vida de desenvolvimento de software, especialmente em equipes que precisam de uma estrutura organizada para trabalhar com novas funcionalidades, releases e correções de bugs críticos. Embora possa ser um pouco mais complexo em comparação a fluxos mais simples (como GitHub Flow), Gitflow proporciona um nível de organização e controle que pode ser essencial para equipes maiores ou projetos complexos.
