---
layout: post
title: "Como sincronizar um fork com seu repositório original"
date: 2015-02-23 15:06:17 -0300
author: Leandro Cavalcante
comments: true
share: true
image:
  feature: http://www.jsenv.com/post_images/OAIDO9GSRD.jpg
categories: git
---

Estamos acostumados a dar um fork em repositórios que contém scripts, aplicações e frameworks para utilizarmos no nosso dia-a-dia, facilitando a criação de nossas aplicações.

Mas, e se o repositório original do qual fizemos o fork se atualizar? Como faremos para sincronizar nosso fork com o repositório original?

<!-- more -->

Vamos entender e resolver estas questões!

### Configurando um relacionamento entre seu fork e o repositório original

Para podermos sincronizar as alterações do repositório original com o seu fork, precisamos seguir os seguintes passos:

1 - Abra o _terminal_ (para usuários Mac e Linux) ou a linha de comando (para usuários Windows).

2 - Liste o repositório corrente configurado no seu fork.

``` 
$ git remote -v
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

3 - Especifique uma ligação para o repositório que será sincronizado ao seu fork.

```
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

4 - Verifique a nova ligação que você especificou para o seu fork.

```
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

Agora que executamos a configuração para sincronizar o fork com o repositório, vamos para a próxima etapa!

### Sincronizando o fork 

Para deixar seu fork sempre atualizado em relação ao reposiório original, siga os passos abaixo:

1 - Abra o _terminal_ (para usuários Mac e Linux) ou a linha de comando (para usuários Windows).

2 - Vá até o diretório do seu projeto (fork)

3 - Busque as _branches_ e seus respectivos _commits_ do repositório. Os _commits_ feitos na branch _master_ serão armazenadas na branch local: _upstream/master_.

```
git fetch upstream
remote: Counting objects: 75, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 62 (delta 27), reused 44 (delta 9)
Unpacking objects: 100% (62/62), done.
From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
 * [new branch]      master     -> upstream/master
``` 

4 - Certifique-se de que está na branch _master_

```
git checkout master
Switched to branch 'master'
```

5 - Faça um _merge_ das suas alterações armazenadas em _upstream/master_ dentro da sua branch _master_ local. Isso atualizará a branch do seu fork sem perder suas atualizações locais.

```
git merge upstream/master
Updating a422352..5fdff0f
Fast-forward
 README                    |    9 -------
 README.md                 |    7 ++++++
 2 files changed, 7 insertions(+), 9 deletions(-)
 delete mode 100644 README
 create mode 100644 README.md
```

Se sua branch local não conter nenhum _commit_, o GIT irá realizar um _"fast-forward"_:

```
git merge upstream/master
Updating 34e91da..16c56ad
Fast-forward
 README.md                 |    5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
```

Pronto! Fork sincronizado e atualizado!

#### Referência:

- **GitHub**: https://help.github.com/articles/syncing-a-fork/
