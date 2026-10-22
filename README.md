# Trabalho DevOps — GitHub Actions, Changelog e Release

## Integrantes

- **Gladson Coronado dos Santos** — RA: 250293
- **Gustavo Francisco dos Santos** — RA: 250296

## 1. Objetivo do trabalho

Este trabalho tem como objetivo praticar a automação de tarefas de desenvolvimento de software utilizando **GitHub Actions**, com foco na geração automática de changelog e na criação e publicação de releases.

Foram desenvolvidos workflows para demonstrar a execução de automações a partir de eventos do GitHub, a atualização do histórico de alterações e a geração de pacotes para distribuição do projeto.

## 2. GitHub Actions

O GitHub Actions foi utilizado para automatizar tarefas por meio de arquivos de configuração YAML armazenados no diretório `.github/workflows/`.

O projeto possui três workflows:

- `hello.yml`: demonstra o funcionamento básico do GitHub Actions.
- `changelog.yml`: gera e atualiza automaticamente o arquivo `CHANGELOG.md`.
- `release.yml`: cria um pacote ZIP e o anexa à release publicada no GitHub.

### 2.1. Workflow CI — `hello.yml`

Foi criado um workflow introdutório para demonstrar a execução de comandos automatizados.

**Gatilhos configurados:**
- `push` para a branch `main`.
- Eventos de `pull_request` direcionados à branch `main`.
- `workflow_dispatch`, que permite a execução manual.

O workflow utiliza uma máquina virtual Ubuntu, obtém o código do repositório e executa comandos de exemplo no terminal.

## 3. Changelog — `changelog.yml`

Foi implementada uma automação para gerar o arquivo `CHANGELOG.md` a partir do histórico de commits do Git.

### O que foi desenvolvido

1. Configuração do workflow para execução após um `push` na branch `main` ou por acionamento manual.
2. Obtenção do histórico completo do repositório utilizando `actions/checkout`.
3. Leitura dos commits recentes por meio do comando `git log`.
4. Formatação automática das informações de cada commit.
5. Geração e atualização do arquivo `CHANGELOG.md`.
6. Verificação de alterações para evitar commits desnecessários.
7. Criação de um commit automático e envio da atualização ao repositório remoto quando o changelog é modificado.

### Informações registradas

Cada entrada do changelog contém:

- Data do commit.
- Mensagem do commit.
- Nome do autor.
- Identificador curto do commit.

O workflow ignora commits de merge e limita a listagem aos 50 commits mais recentes.

Para permitir a atualização automática do arquivo, foi configurada a permissão `contents: write`.

Dessa forma, o histórico de alterações é gerado automaticamente, reduzindo a necessidade de atualização manual da documentação.

## 4. Release — `release.yml`

Foi implementada uma automação para criar e anexar um pacote ZIP à release publicada no GitHub.

### O que foi desenvolvido

1. Configuração do workflow para ser acionado pelo evento `release.published`.
2. Obtenção dos arquivos do repositório.
3. Geração de um arquivo ZIP com o nome baseado na tag da versão.
4. Exclusão dos diretórios `.git` e `.github` do pacote.
5. Upload do arquivo ZIP para a release correspondente, utilizando o GitHub CLI.
6. Configuração do token `GITHUB_TOKEN` e da permissão `contents: write` para permitir o envio do pacote.

### Exemplo de funcionamento

Quando uma release com a tag `v1.1.0` é publicada, o workflow gera o seguinte arquivo:

`trabalho-devops-v1.1.0.zip`

O arquivo é anexado à release, ficando disponível para download na página de versões do repositório.

A automação facilita a distribuição do projeto e evita que o pacote precise ser criado e anexado manualmente.

## 5. Versionamento e releases publicadas

O projeto possui as seguintes versões publicadas:

- **v1.0.0:** primeira versão do projeto, com a aplicação CRUD, Docker e as automações iniciais.
- **v1.1.0:** versão com melhorias na documentação de execução e do GitHub Actions.

As releases permitem identificar versões específicas do projeto e disponibilizar os arquivos correspondentes para download.

O versionamento utiliza o formato `MAJOR.MINOR.PATCH`, no qual:

- `MAJOR`: mudanças incompatíveis.
- `MINOR`: novas funcionalidades compatíveis.
- `PATCH`: correções compatíveis.

## 6. Tecnologias utilizadas

- Git e GitHub.
- GitHub Actions.
- YAML.
- GitHub CLI.
- Markdown.
- Docker e Docker Compose, como complemento para execução da aplicação.

## 7. Extra: aplicação com Docker

Além do foco principal em GitHub Actions, Changelog e Release, o projeto inclui uma aplicação web CRUD executada com Docker Compose, aproveitando os conhecimentos desenvolvidos no primeiro bimestre.

O ambiente utiliza PHP, MySQL e phpMyAdmin.

Para iniciar os serviços, execute na raiz do projeto:

```bash
docker compose up -d --build
```

A aplicação fica disponível em `http://localhost:8080` e o phpMyAdmin em `http://localhost:8081`.

## 8. Resultados obtidos

Com a implementação dos workflows, foi possível demonstrar:

- A execução automatizada de tarefas pelo GitHub Actions.
- A geração do changelog a partir do histórico de commits.
- A atualização automática do arquivo `CHANGELOG.md`.
- A criação de pacotes ZIP associados às releases publicadas.
- A disponibilização de versões do projeto para download pelo GitHub.

O trabalho demonstra conceitos introdutórios de automação de processos, documentação de alterações e distribuição de versões de software.

---

**Trabalho acadêmico — DevOps**
