# gitlab-api-shellscript
Script desenvolvida em bash para criar Grupo, Projeto, Issue, Labels e Milestones no Gitlab On-premise ou em SAAS, alem de permitir a guarda e criação de um repositório no Nexus, criação de um repositório padrão com (.gitlab-ci.yml, mkdocs,  templates do gitlab e grupo de codeowners).

## Configuração
Como trabalhamos com inúmeros usuários, em nosso gitconfig global, incluimos as configurações de nossos usuários.
```
[include]
    path = ~/gitconfig/.gitconfig_labsaas
    path = ~/gitconfig/.gitconfig_labopre
```

Observação: O token da sessão Nexus e OTRS são obtidos através: usuario:senha.

```
echo "usuario:senha"            | openssl enc -base64
```

### Seção Gitlab
```
git config --global gitlab.url   = "http://nome_do_host/api/v4"
git config --global gitlab.token = "spmxPxExemploToken"
```
### Seção Nexus
```
git config --global nexus.url    = "http://nome_do_host:8591"
git config --global nexus.token  = "spmxPxExemploToken"
git config --global nexus.grupo  = "br.com.xxxx.yyyy"
```

### Seção OTRS
```
git config --global otrs.url     = "http://nome_do_host"
git config --global otrs.token   = "spmxPxExemploToken"
```
### Seção ChangeLog
```
git config --global  changelog.format = "%x09 %ad %d %s (%aN)"
git config --global  changelog.mergeformat = '  * %s%n%w(64,4,4)%b'
```
## Função
```
$ git devops --help

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
### Arquivos de Carga de Projetos
| DPDTIC | NOMEDOPROJETO | INICIO | TERMINO | DESCRICAO |
| -----  | ---- | ---- | ---- | ---- | 
|        |      |      |      |      |

### Arquivos de Carga de Labels
| Label | Descrição  | Cor | 
| -----  | ---- | ---- |
|        |      |      |


### Parametros do gitconfig global

```
[user]
	name = root
	email = horacio.vasconcellos@gmail.com
	signingkey = 5CBF2E95E5EB37ED
[core]
	editor = vi
	excludesfile = /home/horacio/.gitignore_global
	quotepath = false
[color]
	diff = auto
	interactive = auto
	status = auto
	branch = auto
[commit]
	template = /home/horacio/gitconfig/mensagem/.commitmensagem
	gpgsign = true
[alias]
	st = status
	bbts = !
	c = commit
	a = add .
	p = push
	s = status
    pbc = !git log --oneline | head -1 | awk '{print $1}' | pbcop
	fr = !git fetch && git rebase -i origin/master
    fixup = "!f() { TARGET=$(git rev-parse "$1"); git commit --fixup=$TARGET ${@:2} && EDITOR=true git rebase -i --autostash --autosquash $TARGET^; }; f"
	details = cat-file -p
	g = grep
	hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
    hor  = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    loga = log --decorate --graph --abbrev-commit --date=relative
	last = log -1 HEAD
    lessdiff = -c core.pager=less diff
	master = checkout master
    mr = !sh -c 'git fetch $1 merge-requests/$2/head:mr-$1-$2 && git checkout mr-$1-$2' -
	path = rev-parse --show-toplevel
	root = rev-parse --show-cdup
	unstage = reset HEAD --
	alias = config — get-regexp ^alias\\.
	amend = commit — amend
	ap = add — patch
	pf = push — force-with-lease
	showconf = !git ls-files --unmerged | cut -f2 | sort -u
	today = log --pretty=format:\"%Cred%h %Cgreen%cd%Creset | %s%C(auto)%d %Cgreen[%an]%Creset\" --date=local --since=midnight
	unmerged = !git ls-files --unmerged | cut -f2 | sort -u
	upstream = rev-parse --symbolic-full-name --abbrev-ref=strict HEAD@{u}
	edtcon = "!f() { git ls-files --unmerged | cut -f2 | sort -u ; }; $EDITOR `f`"
	fodase = reset — hard
	standup = "!git log --all --no-merges --graph --date=relative --committer=$(git config --get user.email) --pretty=format:\"%C(cyan) %ad %C(yellow)%h %Creset %s %Cgreen%d\" --since=$(if [[ Mon == $(date +%a) ]]; then echo last friday; else echo yesterday; fi)"
	chs = !git checkout && git status
	lg-ascii = log --graph --pretty=format:'%h -%d %s (%cr) <%an>' --abbrev-commit
	logs = log --show-signature
	cis = commit -S
[sendemail]
  # https://security.google.com/settings/security/apppasswords
	smtpEncryption = tls
	confirm = auto
	smtpServer = smtp.gmail.com
	smtpUser = contadogmail@gmail.com
	smtpServerPort = 587
	smtppass = "token_gmail"
	suppresscc = self
    transferEncoding = "8bit"
[init]
	defaultBranch = master
[http]
	sslVerify = false
[lfs]
	batch = false
	allowincompletepush = true
	locksverify = true
	contenttype = false
[filter "lfs"]
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
	required = true
    
[rebase]
	autosquash = true
[fetch]
	prune = true
    
[gitlab]
	url = http://nome_do_host/api/v4
	token = PfV9XzErwAaVscs2fzq2

[nexus]
	url = http://nome_do_host:8591
	token = "token_nexus"
	grupo = br.com.nome_do_host
    
[otrs]
  url = http://nome_do_host:8283
  token = "token_otrs"

[push]
	default = simple
	followTags = true
[changelog]
        format = "%x09 %ad %d %s (%aN)"
        mergeformat = '  * %s%n%w(64,4,4)%b'
[gpg]
	program = gpg2
```

