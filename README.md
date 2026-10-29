# Trabalho DevOps — GitHub Actions, Changelog e Release

## Integrantes

- Gladson Coronado dos Santos — RA 250293
- Gustavo Francisco dos Santos — RA 250296

## Sobre o projeto

O objetivo deste projeto é demonstrar o uso do GitHub Actions para automatizar tarefas relacionadas ao versionamento e à organização de um projeto.

Foram criadas duas automações principais: uma responsável por gerar o histórico de alterações do projeto (Changelog) e outra responsável por preparar e anexar um pacote ZIP quando uma nova versão é publicada (Release).

Também foi criado um workflow simples de CI como complemento, para demonstrar a execução de comandos pelo GitHub Actions.

## 1. Changelog — `changelog.yml`

O Changelog foi desenvolvido para registrar automaticamente as alterações realizadas no projeto, utilizando as mensagens dos commits enviados ao repositório.

### Busca do histórico de commits

```yaml
with:
  fetch-depth: 0
```

Esse trecho permite que o workflow tenha acesso ao histórico completo de commits do repositório. Dessa forma, ele consegue consultar as alterações anteriores para montar o arquivo de changelog.

### Geração do arquivo CHANGELOG.md

```bash
git log -n 50 --no-merges --date=short \
  --pretty=format:"- %ad | %s (%an) [%h]" \
  -- . ':(exclude)CHANGELOG.md'
```

Aqui definimos como as alterações serão buscadas e organizadas. O comando consulta os últimos 50 commits, desconsidera os commits de merge e apresenta a data, a mensagem, o autor e o identificador de cada alteração.

Também deixamos o próprio `CHANGELOG.md` fora da consulta para evitar que as atualizações desse arquivo apareçam no histórico que ele mesmo gera.

### Atualização automática do arquivo

```bash
git add CHANGELOG.md
if git diff --cached --quiet; then
  echo "Changelog já está atualizado."
else
  git commit -m "docs: atualiza CHANGELOG [skip ci]"
  git push
fi
```

Depois de gerar o changelog, esse trecho verifica se houve alguma mudança no arquivo. Caso tenha ocorrido, cria um commit e envia a atualização para o GitHub. Se o conteúdo já estiver atualizado, nenhuma nova alteração é enviada.

A identificação `[skip ci]` também ajuda a evitar novas execuções de workflows compatíveis com essa configuração por causa do commit automático do changelog.

## 2. Release — `release.yml`

A automação de Release foi criada para gerar um pacote compactado do projeto e anexá-lo automaticamente à versão publicada na seção Releases do GitHub.

### Geração do pacote ZIP

```bash
zip -r "trabalho-devops-${{ github.event.release.tag_name }}.zip" . -x ".git/*" ".github/*"
ls -lh *.zip
```

Esse trecho reúne os arquivos do projeto em um arquivo ZIP. O nome do pacote utiliza a tag da versão publicada no GitHub. Por exemplo, ao publicar a versão `v1.1.0`, o arquivo gerado será `trabalho-devops-v1.1.0.zip`.

As pastas `.git` e `.github` são ignoradas porque não são necessárias para o pacote final. O comando `ls -lh` permite conferir o arquivo gerado e seu tamanho.

### Envio do pacote para a Release

```yaml
env:
  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

```bash
gh release upload "${{ github.event.release.tag_name }}" trabalho-devops-*.zip --clobber
```

Depois que o pacote é gerado, esse trecho utiliza o GitHub CLI para anexar o arquivo ZIP à Release correspondente. O token disponibilizado pelo próprio GitHub permite que a automação realize essa operação no repositório.

Com isso, o pacote fica disponível como anexo da versão publicada, sem precisar realizar o envio manualmente.

## 3. Tecnologias utilizadas

- Git e GitHub para versionamento e hospedagem do projeto.
- GitHub Actions para executar as automações.
- YAML para configurar os workflows.
- GitHub CLI para anexar o pacote à Release.
- Docker e Docker Compose como complemento do projeto.

## 4. Resultados

Com as automações, conseguimos gerar o histórico de alterações do projeto e preparar pacotes automaticamente para as versões publicadas.

As execuções podem ser acompanhadas pela aba Actions do GitHub, enquanto os arquivos gerados e as versões publicadas ficam disponíveis no próprio repositório.
