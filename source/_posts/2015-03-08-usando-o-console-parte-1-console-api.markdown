---
layout: post
author: Guilherme Moura Nascimento
title: "Usando o Console Parte 1 - Console API"
date: 2015-03-08 03:41:33 +0000
share: true
comments: true
image:
  feature: http://jsenv.com/post_images/72AF51EA3B.jpg
categories: [javascript, debug, console] 
---

[Console](https://developer.chrome.com/devtools/docs/console) é uma ferramenta sagrada para Front-enders, usada para debugar, procurar por erros, gerar logs, fazer testes de performance, teste de asserção, entre outras coisas que nos ajudam fazer analises a aplicação do lado do cliente.

Esse poste tem o objetivo de mostrar algumas dicas que vão facilitar suas analises e debug da aplicação especificamente para o **Google Chrome.**

O Chrome possui uma ferramenta muito poderosa chamada [Console](https://developer.chrome.com/devtools/docs/console), tal é composta por duas APIs, [Console API](https://developer.chrome.com/devtools/docs/console) que será abordada nesse poste e [Comman Line API](https://developer.chrome.com/devtools/docs/commandline-api) que será aborda na parte dois do post.

Lembrando que os exemplos desse post foram feitos no **Google Chrome,** porém são similares em outros browsers(Safari, Firefox e Opera).

**Let's Rock**

## Abrindo Console

Para ter acesso ao Console use as teclas de atalho **Ctrl + Shift + j** no Windows/Linux ou **Command + Option + j** para Mac.

![alt text]( path da imagem "alt da imagem")

### Logando mensagens 

#### Exibir mensagem - *console.log(object [, object, ...])*

Provavelmente o método mais utilizado da API, com ele podemos exibir mensagens no console.

![alt text]( path da imagem "alt da imagem")

Podemos usar coringas para concatenar com variáveis de forma elegante:

| Coringa       | Tipo          |
| ------------- |:-------------:|
| %s            | string        |
| %d ou %i      | integer       |
| %f            | float         |
| %o            | DOM Elements  |
| %O            | Javascript object|
| %c            | CSS style     |

**Concatenando com variáveis:**

![alt text]( path da imagem "alt da imagem")

**Estilizando as mensagens:**

![alt text]( path da imagem "alt da imagem")

#### Exibir mensagens de alerta - *console.warn(object [, object, ...])*

![alt text]( path da imagem "alt da imagem")

#### Agrupando logs - *console.group(object [, object, ...])* ,* console.groupEnd()*

![alt text]( path da imagem "alt da imagem")

#### Agrupando com  groupCollapsed -  *console.groupCollapsed(object [, object, ...])*

![alt text]( path da imagem "alt da imagem")

### Testes

É possível usar o **Console** para efetuar alguns teste simples, como teste de asserção, tempo de execução de trechos de código e track de eventos.

#### Testando uma condição - *console.assert(expression, object)*

Você pode usar o método `assert` para testar uma condição, caso ele seja falsa, será exibida a mensagem.

![alt text]( path da imagem "alt da imagem")

#### Contador - *console.count(label)*

Exibe a quantidade de vezes que a mesma `label` foi invocada.

![alt text]( path da imagem "alt da imagem")

#### Calculando o tempo - *console.time(label) e console.timeEnd(label)*

Contabilizando o tempo de execução de trechos de códigos, onde cada label está relacionada a um timer.

![alt text]( path da imagem "alt da imagem")

Console API possui vários métodos, tire um tempinho para ler a [documentação completa](https://developer.chrome.com/devtools/docs/console-api).

Safari, Firefox, Opera e Internet Explorer também possuem seus próprios Console, seguei o link da documentação de cada um:

**Firefox**: [http://goo.gl/E3o0kK](http://goo.gl/E3o0kK) <br/>
**Safari**: [http://goo.gl/07sSft](http://goo.gl/07sSft) <br/>
**Opera**: [http://goo.gl/gaEtGr](http://goo.gl/gaEtGr) <br/>
**Internet Explorer**: [http://goo.gl/uzPYZZ](http://goo.gl/uzPYZZ)


