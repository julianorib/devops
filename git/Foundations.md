## Week 1 - Introdução ao Git e GitHub

- Git: Sistema de controle de versão distribuído que rastreia mudanças no código.
- GitHub Entities: Componentes como repositórios, branches, commits e PRs.
- GitHub Markdown: Linguagem de marcação usada para formatar texto em GitHub (README, Issues).
- GitHub Desktop/Mobile: Aplicativos para gerenciar repositórios localmente ou em dispositivos móveis.
- Diferença entre Git e GitHub: Git é a ferramenta de versionamento; GitHub é a plataforma de hospedagem e colaboração.
- Repositório GitHub: Local onde o código e o histórico de versões são armazenados.
- Commit: Salva mudanças no código, criando um ponto no histórico de versões.
- Branching: Criar uma linha de desenvolvimento independente de código.

## Week 2 - Trabalhando com Repositórios GitHub

- Gerenciamento de repositório: Configurar e gerenciar repositórios (permissões, colaboradores).
- Criar um novo repositório: Iniciar um novo projeto no GitHub.
- Templates de repositório: Modelos predefinidos para criar repositórios com configuração inicial.
- Clonar um repositório: Baixar uma cópia local de um repositório remoto.
- Criar um novo branch: Iniciar uma nova linha de desenvolvimento paralela.
- Adicionar arquivos ao repositório: Subir novos arquivos ou modificar existentes.
- Ver insights do repositório: Estatísticas sobre contribuições e atividades.
- Feature previews: Recursos em fase de teste no GitHub.

## Week 3 - Recursos de Colaboração
- Issues: Ferramenta de acompanhamento de bugs, tarefas e discussões.
- Pull Requests (PRs): Solicitações para fundir alterações de código.
- Discussões: Espaço para conversas públicas sobre o projeto.
- Notificações: Alertas sobre atividades relevantes no repositório.
- Gists: Permite compartilhar pequenos trechos de código ou arquivos. Útil para armazenar ou compartilhar scripts, notas ou exemplos de código.
- Pages: Serviço de hospedagem de sites estáticos diretamente de repositórios do GitHub. Ideal para portfólios, blogs ou documentações
- Wikis: Documentação
- Markdown Features: Usar Markdown para formatar conteúdo.
- Linkar PR a um Issue: Conectar PR a um Issue para rastreamento de progresso.
- Atribuir Issues: Designar responsáveis por tarefas ou bugs.
- Milestones:  Usados para agrupar issues e pull requests em etapas maiores de desenvolvimento, ajudando a acompanhar o progresso de um projeto ao longo do tempo.
- Draft PR: PRs ainda em fase de revisão inicial.

## Week 4 - Desenvolvimento Moderno
- GitHub Actions: Automação de fluxos de trabalho (CI/CD).
- GitHub Copilot: Assistente de codificação que sugere linhas de código.
- GitHub Codespaces: Um ambiente de desenvolvimento na nuvem que permite programar diretamente no navegador ou em um editor compatível, como VSCode.
- GitHub.dev: Um ambiente de edição de código online que pode ser acessado diretamente no navegador, útil para edições rápidas em repositórios.

## Week 5 - Gerenciamento de Projetos
- GitHub Projects: Ferramenta de gerenciamento de projetos dentro do GitHub, integrando repositórios e issues com quadros kanban e outras funcionalidades de organização.

## Week 6 - Privacidade, Segurança e Administração
- 2FA (Autenticação de Dois Fatores): Proteção extra para contas GitHub.
- EMUs (Enterprise Managed Users): Contas gerenciadas centralmente por empresas.

## Week 7 - Benefícios da Comunidade GitHub
- Open Source: Software com código-fonte aberto e colaborativo.
- InnerSource: Aplicação de práticas open source dentro de empresas.
- GitHub Sponsors: Recurso para apoiar financeiramente desenvolvedores open source.


## Tabela Comparativo Contas

| **Recurso**                     | **GitHub Free**                     | **GitHub Pro**                      | **GitHub Team**                   | **GitHub Enterprise**            |
|----------------------------------|-------------------------------------|-------------------------------------|-----------------------------------|----------------------------------|
| **Repositórios privados**        | Ilimitados (colaboradores limitados) | Ilimitados                          | Ilimitados                        | Ilimitados                       |
| **Repositórios públicos**        | Ilimitados                          | Ilimitados                          | Ilimitados                        | Ilimitados                       |
| **Colaboradores**                | Limitado a 3 por repositório privado | Ilimitados                          | Ilimitados                        | Ilimitados                       |
| **Actions (CI/CD)**              | 2,000 min/mês                       | 3,000 min/mês                       | 3,000 min/mês                     | 50,000 min/mês + auto-hospedagem |
| **Packages**                     | 500 MB de armazenamento             | 2 GB de armazenamento               | 2 GB de armazenamento             | 50 GB de armazenamento           |
| **Suporte**                      | Comunidade                          | Suporte via e-mail                  | Suporte via e-mail                | Suporte 24/7 (e-mail, telefone)  |
| **Segurança (Dependabot, etc.)** | Básico                              | Completo                            | Completo                          | Completo                         |
| **Insights & Analytics**         | Limitado                            | Completo                            | Completo                          | Completo                         |
| **Gerenciamento de equipes**     | Não incluso                         | Não incluso                         | Gerenciamento básico              | Gerenciamento avançado           |
| **GitHub Pages**                 | Incluído                            | Incluído                            | Incluído                          | Incluído                         |
| **GitHub Projects**              | Incluído                            | Incluído                            | Incluído                          | Incluído                         |
| **GitHub Copilot**               | Não incluso                         | Assinatura separada                 | Assinatura separada               | Assinatura separada              |
| **SAML SSO (Single Sign-On)**    | Não incluso                         | Não incluso                         | Não incluso                       | Incluído                         |
| **Enterprise Managed Users (EMUs)** | Não disponível                     | Não disponível                      | Não disponível                    | Disponível                       |
| **Políticas de segurança personalizadas** | Não disponível                  | Não disponível                      | Não disponível                 | Disponível                       |



## Github Actions Syntax

A sintaxe de workflows no GitHub Actions é baseada em arquivos YAML. Aqui está uma visão geral dos principais elementos:

name: (Opcional) Nome do workflow.

```
name: CI Workflow
```
on: Define os eventos que acionam o workflow (como push, pull request, cron).

```
on: 
  push:
    branches:
      - main
  pull_request:
```
jobs: Conjunto de tarefas (jobs) que serão executadas.
```
jobs:
  build:
    runs-on: ubuntu-latest
```
runs-on: Especifica o ambiente onde o job será executado (ex: ubuntu-latest, windows-latest, macos-latest).
```
runs-on: ubuntu-latest
```
steps: Lista de etapas a serem executadas dentro de um job. Cada etapa pode executar comandos ou ações.
```
steps:
  - name: Checkout code
    uses: actions/checkout@v2
  - name: Run tests
    run: npm test
```
uses: Refere-se a uma ação pré-existente do GitHub Marketplace ou de um repositório público.
```
uses: actions/setup-node@v2
```
run: Executa um comando de shell.
```
run: echo "Hello, World!"
```
env: Define variáveis de ambiente para o job ou para uma etapa específica.
```
env:
  NODE_ENV: production
```
strategy: (Opcional) Permite criar uma matriz de builds para executar jobs com diferentes combinações.
```
strategy:
  matrix:
    node-version: [10, 12, 14]
```
Essa estrutura modular permite definir workflows de forma clara e flexível, facilitando a automação de diversas tarefas no ciclo de desenvolvimento.