---
layout: post
title: "Membros Privados em Javascript"
date: 2015-02-26 15:04:53 -0300
comments: true
author: Leandro Cavalcante
image:
  feature: http://www.jsenv.com/post_images/47DF5E7897.jpg
categories: [javascript, tips]
---

Quem nunca se perguntou: _"Como faço para deixar minhas variáveis e métodos privados com Javascript?"_. Pois bem, tentarei de uma forma bem pragmática mostrar como fazemos isso.

Algumas pessoas acreditam que o Javascript não tem a capacidade de _"esconder informações"_, porquê `object` não pode ter variáveis e métodos privados. Mas isso não passa de um mal-entendido.

#### _Sim! Objetos Javascript podem ter membros privados._

Mas, antes de nos aprofundarmos vamos entender um pouco mais do funcionamento do Javascript em relação aos objetos:

<!--more-->

## Objects

Javascript é fundamentalmente baseado em _objects_. Funções são objetos, arrays são objetos e Objetos são objetos. Mas o que são objetos?

Objetos são coleções de pares nome-valor. Os nomes são _strings_ e o valores podem ser _strings_, _numbers_, _booleans_ e _objects_ (incluindo _arrays_ e _funções_).

## Membros Públicos

Os membros de um `object` são, sem exceção públicos. Qualquer função pode acessar, modificar, deletar ou até mesmo adicionar novos membros. Existem duas maneiras principais de inserir membros em um novo objeto:

### No construtor

Esta técnica é usualmente utilizada para iniciar instâncias de _variáveis_ públicas. A variável `this` do construtor é usada para adicionar um membro ao objeto.

``` javascript
function Adicionar ( param ) {
	this.membro = param;
}
```

Então, se construírmos um novo objeto

``` javascript
var novo_objeto = new Adicionar( 'a-b-c' );
```

O valor de `novo_objeto.membro` será `a-b-c`.

### No prototype

Esta técnica é utilizada para adicionar _métodos_ públicos. Quando um mebro é procurado dentro do próprio objeto e não é encontrado, ele é retirado do protótipo do Construtor do objeto. O mecânismo do _prototype_ é usado para herança. O que também conserva a memória. Para adicionar um método para todos os objetos criados a partir do Construtor do prototype:

``` javascript
Adicionar.prototype.stamp = function ( string ) {
	return this.membro + string;
}
```

Então, podemos chamar o método:

``` javascript
novo_objeto.stamp( '-d-e-f' );
```

E o resultado será: `a-b-c-d-e-f`.

## Membros Privados 

Membros privados são criados pelo construtor. Variáveis comuns e parâmetros do _Construtor_ se tornam membros privados.

``` javascript
function Container ( param ) {
	this.membro = param;
	var limite  = 3,
		that    = this;
}
```

Este construtor criou três variáveis privadas: `param`, `limite` e `that`. Elas estão anexadas ao objeto, mas não estão acessíveis fora, nem são acessíveis aos métodos públicos do objeto. Elas estão acessíveis aos métodos privados. Métodos privados são funções internas do _Construtor_.

``` javascript
function Container ( param ) {
	this.membro = param;
	var limite  = 3,
		that    = this;
	//
	function resgata () {
		if ( limite > 0 ) {
			return true
		} else {
			return false
		}
	}
}
``` 

Por convenção, nós declaramos a variável `that` privada. Isso é usado para tornar o objeto disponível para os métodos privados, pois ao usar o `this` dentro da função ele apontará para o `this` dela mesma e não para o `this` do _Construtor_. 

Métodos privados não podem ser invocados por métodos públicos. Para fazer os métodos privados utilizáveis, nós precisamos fazer com que ele tenha privilégios.

## Membros Privilegiados

Um método privilegiado tem permissão para acessar variáveis e métodos privados, e é acessível por métodos públicos e externos. É possível deletar ou sobrescrever um método privilegiado, mas não é possível alterá-lo e nem forçá-lo a exibir seu conteúdo.

Métodos privilegiados são atribuídos com `this` dentro do _Construtor_.

```javascript
function Container ( param ) {
	this.membro = param;
	var limite  = 3,
		that    = this;
	//
	function resgata () {
		if ( limite > 0 ) {
			return true
		} else {
			return false
		}
	}
	//
	this.escreve = function () {
		return resgata() ? that.membro : null;
	}
}
```

Então temos o método privilegiado `escreve` que acessa os valores definidos dentro do _Construtor_, porém não pode alterá-los. \o/

Isso só é possível porquê existem as _Closures_ no Javascript.

## Closures

Os padrões de membros `public`, `private` e `privileged` só se tornam possíveis pelo fato da linguagem Javascript possuir _Closures_.

Isso quer dizer que uma função interna sempre tem acesso as variáveis e parâmetros da função externa, mesmo depois dela já ter executado.
Essa é uma propriedade muito poderosa da linguagem Javascript!

Métodos privados e privilegiados só podem ser atribuidos enquanto o objeto está sendo construído, já os públicos podem ser adicionados a qualquer momento.

## Padrões utilizando Closures

### Membros Públicos
``` javascript
function Construtor ( ... ) {
	this.membro = valor;
}
Construtor.prototype.membro = valor;
```

### Membros Privados
``` javascript
function Construtor ( ... ) {
	var that = this,
		nome = valor;
	//
	function nome ( ... ) { ... };
}
```

Observação: a instrução da função

``` javascript
	function nome ( ... ) { ... };
```

é uma abreviação para: 

``` javascript
	var nome = function nome ( ... ) { ... };
```


### Membros Privilegiados
``` javascript
function Construtor ( ... ) {
	this.nome = function ( ... ) { ... }
}
```

Referência:

- ** Douglas Crockford **: http://javascript.crockford.com/private.html
