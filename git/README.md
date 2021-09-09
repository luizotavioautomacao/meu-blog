git config --list
git config --global core.editor code --wait
git config --global --edit
--
git commit --amend --no-edit

---
- [História do GIT](https://youtu.be/6Czd1Yetaac)
##[Aprendendo GIT com BitBucket](https://www.atlassian.com/br/git/tutorials/learn-git-with-bitbucket-cloud)
###[DOCUMENTAÇÃO CONFLUENCE](https://dmaind.atlassian.net/wiki/spaces/NT/overview)
###[BITBUCKET](https://BItbucket.org), [GUIA RÁPIDO GIT](https://rogerdudler.github.io/git-guide/index.pt_BR.html), [stack overflow](https://stackoverflow.com/questions/4114095/how-do-i-revert-a-git-repository-to-a-previous-commit)

* ## Primeiros passos
**`git config --list`**
    **`git config --global user.name "meu-nome"`**
    **`git config --global user.email "meu-email@email.com"`**
    **`git init`**
    **`git clone url`**
    **`.... git remote add origin url`**

* ## Enviando Alterações
    **`git status`**
    **`git add .`**
    **`git commit -m "mensagem"`**
    **`git push -u origin nome`**

* ## Atualizando repositório local
    **`git pull`**
    **`git pull origin/'nome-da-branch'`**

* ## Criar branch
    **`git branch nome`**
    **`git branch`**
    **`git checkout nome`**

* ## Fundindo uma branch na master
    **`git checkout master`**
    **`git merge nome-da-branch`**

* ## Problemas ao trocar de branch:
    **`git ls-remote origin`**
    **`git fetch`**

* ## Renomear uma branch
    **`git branch -m old-name new-name`**

* ## Ignorar mudanças na branch
    **`git checkout  .`**

* ## Mudar de branch remota
  **` git remote set-head origin some_branch`**

* ##Removendo um branch
**` git branch -d nome-da-branch `**
**`git push origin --delete nome-da-branch`**

#### local
git branch -d nome-da-branch

#### remote
git push origin --delete nome-da-branch

* ##Descartar alterações de uma branch
**` git stash save --keep-index --include-untracked `**

---

##Alteração no meio da Branch

##1) commit WIP
git add . -A
git commit -m"WIP: Parei no meio"
git checkout OUTRA_BRANCH

**vá para outra branch e faça o que precisar**
git checkout OUTRA_BRANCH

**volte para branch anterio**
git checkout BRANCH_ANTERIOR

**volte o último commit mas mantenha as informações**
git reset HEAD~1


##2) stash
git stash save "Parei no meio"

**vá para outra branch e faça o que precisar**
git checkout OUTRA_BRANCH

**volte para branch anterior**
git checkout BRANCH_ANTERIOR

**recupere o último stash**
git stash list
git stash pop

---
### Voltar [commit](https://pt.stackoverflow.com/questions/19393/como-voltar-o-projeto-a-um-commit-espec%C3%ADfico) a uma branch específica

- **This will destroy any local modifications.**
- **Don't do it if you have uncommitted work you want to keep.**

```
git log
git reset --hard 036b56602e22b7c4e4462504a26e50004a1cd2cc
```

### Alternatively, if there's work to keep:
```
git stash
git reset --hard 0d1d7fc32
git stash pop
```

**Temporarily switch to a different commit**
If you want to temporarily go back to it, fool around, then come back to where you are, all you have to do is check out the desired commit:

This will detach your HEAD, that is, leave you with no branch checked out:
```
git checkout 0d1d7fc32
```
**Or if you want to make commits while you're there, go ahead and make a new branch while you're at it:**
```
git checkout -b old-state 0d1d7fc32
```

---
## Branch do SERVIDOR
- **master** -> *Manfline + Luiz Otávio*
- **luizotavio-branch** -> *rodando no servidor*
- **luizotavio-branch-diagrama** -> *desenvolvendo agora*

---

##Para remover usuário do git local pelo terminal 
 - [tutorial aqui](https://sobrelinux.info/questions/283763/how-to-switch-git-user-at-terminal)

----
- Mandar do repositório local para projeto recém criado


```
git clone -b <branch> <remote_repo>

git clone -b develop git@github.com:user/myproject.git

```
git remote add origin https://your-username@bitbucket.org/ommunedevelopers/repository-name.git

(why-you-should-use-git-pull-ff-only-git-is-a)[https://blog.sffc.xyz/post/185195398930/why-you-should-use-git-pull-ff-only-git-is-a]

