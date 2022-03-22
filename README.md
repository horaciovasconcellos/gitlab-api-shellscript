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
### Sessão Gitlab
git config --global gitlab.url  = http://horaciovasconcellos.com.br
git config --global gitlab.token = spmxPxExemploToken

### Sessão Nexus
git config --global nexus.url   = http://horaciovasconcellos.com.br:8591
git config --global nexus.token = spmxPxExemploToken
git config --global nexus.grupo = "br.com.xxxx.yyyy"

### Sessão OTRS
git config --global otrs.url    = http://horaciovasconcellos.com.br
git config --global otrs.token  = spmxPxExemploToken

### Sessão ChangeLog
git config --global  changelog.format = "%x09 %ad %d %s (%aN)"
git config --global  changelog.mergeformat = '  * %s%n%w(64,4,4)%b'


