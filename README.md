## CICD

Repositorio base

https://github.com/jrballot/CICD

### GIT

'''

Instalando

> yum install git -y -q

-----------------------------------------------------------------
mkdir git 

Criar repo local

> git init

-----------------------------------------------------------------
Informando/Configurando user

> git config --global user.name "Bruno Mendes"
> git config --global user.email bml@4linux.com

-- Git congif e a chave para utilizar todas as infos.
	hierarquias : System(a maquina que estou rodando) > global(ao repo) > local(associado ao user)

-----------------------------------------------------------------
Mostrar as configuracoes que realizei:

> git config --list

* Bare eh um gitlab da vida sem gerenciador, mas voce eh o gerenciador da porra toda.

-----------------------------------------------------------------
Trocar o editor default:

> git config --global core.editor vim
-----------------------------------------------------------------
> touch arq00
> git status - status do commit e dos arquivos
> git add arq00
> git status 
> git rm --cached arq00
> > git add arq00
> git commit -m "initial"
> git log
> git diff -- mostra a diferenca
> git commit -a -m "texto added"
> git blame arq00 - culpado da alteracao		
-----------------------------------------------------------------
Git GC

> Limpa arquivos nao necessarios no repo local.
https://git-scm.com/docs/git-gc
-----------------------------------------------------------------
Desfazendo as merdas:

*Checkout
> echo CICD >> arq00
> git checkout arq00 -- (limpei minha alteracao no meu arquivo que acabei de usar, ele volta o estado do commit.)

*Reset
> echo CICD >> arq00
> git comit -a -m "nome do curso adicionado"
> git log
> git log --color --oneline --decorate
> git reset HEAD~1 --- (Estou solicitando para voltar o commit anterior e removo o atual, mas nao removo do meu arquivo)

*Revert - revert as alteracoes de um commit realizado em outro commit -- preciso informar qual commit estou revertendo

> git commit -a -m "voltando curso CICD para arq00"
> git revert e742348
> git revert --continue
> git add arq00
> git revert --continue
> git log
-----------------------------------------------------------------

-----------------------------------------------------------------

-----------------------------------------------------------------

-----------------------------------------------------------------

'''

