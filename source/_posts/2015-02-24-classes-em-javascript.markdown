---
layout: post
title: "Classes em Javascript"
date: 2015-02-24 00:01:27 -0300
author: Rodrigo Fernandes
comments: true
categories: [javascript, prototipos] 
image:
  feature: http://www.jsenv.com/post_images/0A7ED815C4.jpg
---
	
Quando comecei a desenvolver em JavaScript um dos primeiros conflitos que tive com a linguagem foi o famoso caso das classes, na faculdade eu havia aprendido os conceitos de orientação a objeto e aplicado esses conhecimentos somente nas linguagens Java e C#, de início foi difícil compreender esse mundo novo do JavaScript, afinal tanto em Java como em C# não se tem os protótipos, por este motivo quis escrever este post, para aqueles que estão iniciando possam  compreender melhor o funcionamento da linguagem , mas vamos por partes, para compreender classes em JavaScript primeiro é necessário compreender o que é protótipos.

<!-- more -->

## Protótipos

Todo objeto em JavaScript tem um segundo objeto, este segundo objeto é chamado de protótipo, e o primeiro sempre herda as propriedades deste protótipo. Todos os objetos criados com a palavra-chave `new` utilizam a função construtora como protótipo, exemplo:

``` javascript criando objeto
//criando objeto e herdando de Date.prototype
var data = new Date();
//utilizando método do objeto new Date() que foi herdada pelo objeto data
data.getDate();
```

O objeto data herda as propriedades de `Date.prototype` e de `Object.prototype`, essa série de encadeamento nos chamamos de encandeamento de protótipos.

## Criando classes em JavaScript

A maneira como se é criada classes em JavaScript é diferente da maneira como é feita em Java, as classes em JavaScript são baseadas em cima do mecanismo de  protótipos, primeiro é necessário criar uma função que se tem o nome de construtora e através dela  é possível se realizar a herança de propriedades para o objeto que vai herdar, segue o código abaixo tanto em JavaScript como em Java para que você possa comparar as diferenças.

``` java exemplo classe pessoa java
//classe em Java
public class Pessoa {
	//atributos	
	private String nome;
	private int idade;
	//construtor
	public Pessoa(String nome, int idade){
		this.nome = nome;
		this.idade = idade;
	}
	//metodos
	public void falar(){
		System.out.print("Meu nome é " + this.nome + " e minha idade é " + this.idade);
	}
	public void andar(){
		System.out.println(this.nome + " esta andando");
	}
	
	
	public static void main(String[] args){
		Pessoa meuObjeto = new Pessoa("Rodrigo", 30);
		meuObjeto.andar();
		meuObjeto.falar();
		
	}
}
```

``` javascript exemplo classe pessoa javascript
//funçao construtora
function Pessoa (nome, idade){
	this.nome = nome;
	this.idade = idade;
}

Pessoa.prototype = {
	falar: function(){
		console.log('meu nome e ' + this.nome + ' minha idade e ' + this.idade);
	},
	andar: function(){
		console.log(this.nome +' esta andando');
	}
}
```

A maneira como lidamos, como instanciamos e utilizamos os métodos em JavaScript são os mesmos de linguagem Java.

``` javascript exemplo criando objeto em javascript
//criando objeto em JavaScript
var meuObjeto = new Pessoa('Rodrigo', 30);
//invocando os metodos
meuObjeto.falar();
meuObjeto.idade();	
``` 
