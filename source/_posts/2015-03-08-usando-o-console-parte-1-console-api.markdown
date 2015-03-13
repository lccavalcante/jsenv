---
layout: post
author: Guilherme Moura Nascimento
title: "Usando o Console Parte 1 - Console API"
date: 2015-03-08 03:41:33 +0000
share: true
comments: true
image:
  feature: /post_images/72AF51EA3B.jpg
  content: "/post_images_content/usando_o_console_parte_um"
categories: [javascript, debug, console] 
---

[Console](https://developer.chrome.com/devtools/docs/console) é uma ferramenta sagrada para Front-enders, usada para debugar, procurar por erros, gerar logs, fazer testes de performance, teste de asserção, entre outras coisas que nos ajudam a fazer análises a aplicação do lado do cliente.

Esse poste tem o objetivo de mostrar algumas dicas que vão facilitar suas análises e debug da aplicação especificamente para o **Google Chrome.**
<!--more-->

O Chrome possui uma ferramenta muito poderosa chamada [Console](https://developer.chrome.com/devtools/docs/console), tal é composta por duas APIs, [Console API](https://developer.chrome.com/devtools/docs/console) que será abordada neste poste e [Comman Line API](https://developer.chrome.com/devtools/docs/commandline-api) que será abordada na parte dois do post.

Lembrando que os exemplos deste post foram feitos no **Google Chrome,** porém são similares em outros browsers (Safari, Firefox e Opera).

**Let's Rock**

## Abrindo o Console

Para ter acesso ao Console use as teclas de atalho **Ctrl + Shift + j** no Windows/Linux ou **Command + Option + j** para Mac.

![Console]({{ page.image.content }}/console.jpg)

### Logando mensagens 

#### Exibir mensagem - *console.log(object [, object, ...])*

Provavelmente é o método mais utilizado da API, com ele podemos exibir mensagens no console.

``` javascript
var a = 10, b = 20;

console.log(a)
console.log(b)
```

![Logando mensagens]({{ page.image.content }}/logando-mensagem.jpg)

Podemos usar curingas para concatenar com variáveis de forma elegante:

| Curinga       | Tipo          |
| ------------- |:-------------:|
| **%s**            | string        |
| **%d ou %i**      | integer       |
| **%f**            | float         |
| **%o**            | DOM Elements  |
| **%O**            | Javascript object|
| **%c**            | CSS style     |

**Concatenando com variáveis:**

``` javascript
var a = 10, b = 20;

console.log("valor de a: %i e valor de b: %i", a, b)
```

![Concatenando]({{ page.image.content }}/logando-concat-string.jpg)

**Concatenando com objetos:**

``` javascript
var nome  = { nome: "Geremias", cidade:"Jaraitinga" }
var curso = { nome: "Administração", turma: "A" }

console.log("Nome: %O e curso: %O", nome, curso)
```

![Logando objetos]({{ page.image.content }}/logando-objetos.jpg)

**Estilizando as mensagens:**

``` javascript
console.log("%cHello %cworld","color:red;font-size:x-large","color:blue")
```

![Logando com estilo]({{ page.image.content }}/logando-estilizado.jpg)

#### Exibir mensagens de alerta - *console.warn(object [, object, ...])*

``` javascript
console.warn("E-mail inválido")
```

![Logando mensagem de warn]({{ page.image.content }}/logando-warn.jpg)

#### Agrupando logs - *console.group(object [, object, ...])*, *console.groupEnd()*

``` javascript
console.group("Logs de Usuários")
console.log("Usuário logado")
console.log("Usuário efetuo a compra com sucesso")
console.groupEnd();

console.group("Carrinho de compras")
console.log("Carrinho vazio")
console.log("Novo item adicionado ao carrinho")
console.groupEnd()
```

![Agrupando]({{ page.image.content }}/logando-group.jpg)

#### Agrupando com  groupCollapsed -  *console.groupCollapsed(object [, object, ...])*

``` javascript
console.groupCollapsed("Logs de Usuários")
console.log("Usuário logado")
console.log("Usuário efetuo a compra com sucesso")
console.groupEnd()

console.groupCollapsed("Carrinho de compras")
console.log("Carrinho vazio")
console.log("Novo item adicionado ao carrinho")
console.groupEnd()
```

![Agrupando com collapsed]({{ page.image.content }}/log-group-groupCollapsed.gif)

### Testes

É possível usar o **Console** para efetuar alguns testes simples, como teste de asserção, tempo de execução e tracking.

#### Testando uma condição - *console.assert(expression, object)*

Você pode usar o método `assert` para testar uma condição, caso ela seja falsa, será exibida a mensagem.

``` javascript
var a = 10, b = 20;

console.assert( a > b, "A não é maior que B")
```

![Testes com assert]({{ page.image.content }}/logando-assert.jpg)

#### Contador - *console.count(label)*

Exibe a quantidade de vezes que a mesma `label` foi invocada.

``` javascript
function validaCampo (campo) {
 console.count('Validando campo ' + campo);
}

validaCampo('phone')
validaCampo('email')
```

![Count]({{ page.image.content }}/log-group-count.gif)

#### Calculando o tempo - *console.time(label) e console.timeEnd(label)*

``` javascript
var users = new Array(10000);
console.time('Tempo para contabilitar os usuários')

for(var i = 0; i < users.length; i+=1) {
 users[i] = new Object()
}
console.timeEnd("Tempo para contabilitar os usuários")
```

Contabilizando o tempo de execução de trechos de códigos, onde cada label está relacionada a um timer.

![Count]({{ page.image.content }}/logando-timer.jpg)

Console API possui vários métodos, tire um tempinho para ler a [documentação completa](https://developer.chrome.com/devtools/docs/console-api).

Safari, Firefox, Opera e Internet Explorer também possuem seu próprio Console, segue o link da documentação de cada um:

**Firefox**: [http://goo.gl/E3o0kK](http://goo.gl/E3o0kK) <br/>
**Safari**: [http://goo.gl/07sSft](http://goo.gl/07sSft) <br/>
**Opera**: [http://goo.gl/gaEtGr](http://goo.gl/gaEtGr) <br/>
**Internet Explorer**: [http://goo.gl/uzPYZZ](http://goo.gl/uzPYZZ)


