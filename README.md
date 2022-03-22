# gitlab-api-shellscript

Criação de um conjunto um software para criar Grupo, Projeto, Issue, Labels no Gitlab On-premise ou em SAAS, alem de permitir a guarda e criação de um repositório no Nexus.

## Configuração

### Como trabalhamos com inúmeros usuários, em nosso gitconfig global, incluimos as configurações de nossos usuários
```
[include]
    path = ~/gitconfig/.gitconfig_abx
    path = ~/gitconfig/.gitconfig_abb
    path = ~/gitconfig/.gitconfig_xxx

```
Observação: O token da sessão Nexus e OTRS são obtidos através: usuario:senha
```
echo "usuario:senha"            | openssl enc -base64
```

### Sessão Gitlab
```
git config --global gitlab.url  = http://horaciovasconcellos.com.br
git config --global gitlab.token = spmxPxExemploToken
```
### Sessão Nexus
```
git config --global nexus.url   = http://horaciovasconcellos.com.br:8591
git config --global nexus.token = spmxPxExemploToken
git config --global nexus.grupo = "br.com.xxxx.yyyy"
```

### Sessão OTRS
```
git config --global otrs.url    = http://horaciovasconcellos.com.br
git config --global otrs.token  = spmxPxExemploToken
```
### Sessão ChangeLog
```
git config --global  changelog.format = "%x09 %ad %d %s (%aN)"
git config --global  changelog.mergeformat = '  * %s%n%w(64,4,4)%b'
```

## Função


```
usage: git-devops -h|help|?

OPTIONS:
  -g, --grupo               Criacao no Gitlab: Grupo                    -g nome_do_grupo (onde: nome_do_grupo = no|path|descricao)
  -p, --projeto             Criacao no Gitlab: Projeto                  -p nome_do_projeto (onde: Grupo obrigatario e nome_do_projeto = nome|descricao|yyyymmdd)
  -f, --file                Criação Projeto (File CSV)                  -f     nome_do_arquivo.csv (IDPDTIC;NOMEDOPROJETO;INICIO;TERMINO;DESCRICAO)
  -l, --label               Criacao no Gitlab: Labels                   -l                 (Tem que passar o grupo e projeto)
  -m, --mile                Criacao no Gitlab: Milestones               -p                 (Tem que passar o grupo e projeto)
  -n, --nexus               Criacao no Nexus.: Repositorio              -n                 (Concatenacao do Grupo e Projeto)
  -i, --issue               Criacao de uma issue (Grupo:Projeto:issue)  -i nome_da_issue   (Tem que passar o grupo e projeto)
  -b, --bump                Lancar uma vesao                            -b diretorio       (Lancamento de versao)
  -c, --clone               Clone do repositorio                        -c protocolo://hostname[:porta]/[caminho/arquivo.git (Clone repositorio + branch developer)
  -r, --merge               Push repositorio+MergeRequest               -r                 (Assume diretorio do projeto)
  -o, --log                 Geracao de Changelog                        -o                 (Tem que passar o grupo e projeto)
  -s, --standard            Criacao projeto padrao                      -s                 (Criara um projeto padrao criara localmente)
  -u, --user                Usuario                                     -u     usuario:senha   (Usuario e senha do repositorio)
  -h, --help, ?             Auxilio/Display Help                        -h                 (Display help)
```

