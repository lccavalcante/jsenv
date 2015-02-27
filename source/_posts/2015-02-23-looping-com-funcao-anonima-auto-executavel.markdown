---
layout: post
title: "Looping com função anônima auto-executável"
date: 2015-02-23 13:55:09 -0300
author: Guilherme Moura Nascimento
comments: true
image:
  feature: http://www.jsenv.com/post_images/C536C62904.jpg
categories: javascript
---

Javascript em alguns momentos é uma linguagem muito traiçoeira, prega peças que consomem horas de debug e paciência. São os chamados [Gotchas](http://en.wikipedia.org/wiki/Gotcha_%28programming%29), na tradução literal, _"pegadinhas"_. 

Coisas que pela lógica deveriam funcionar de uma maneira, mas por características de um sistema ou alguma linguagem programação funcionam de maneira inesperada.
<!-- more -->

Esse post se dedica a tratar de um Gotcha muito comum relacionado a  escopo, que provavelmente você já foi pego por ele, caso não, é hora de explodir sua cabeça.

``` javascript gotcha http://en.wikipedia.org/wiki/Gotcha_%28programming%29#Gotchas_in_JavaScript_programming_language Gotcha (programming)]
var func = [];
for( var i = 0; i < 3; i++ ){
 func[i] = function () {
  alert(i);
 }
}

func[2]();
func[0]();
```
Ao executar esse trecho de código nos deparamos com o alert sempre com o valor 3.

**Oh God, Help me!**

Take it easy boy, vamos entender o que acontece. 

O `i` dentro closure aponta para `i` global.  Quando chamamos a função `func[0]()` o `i` será 3 por que o valor do `i` global é 3.

Problema semelhante acontece quando colocamos um ajax em um looping, e desejamos usar o índice dentro de seus callbacks. O índice aparece como `undefined`, veja o exemplo:


``` javascript ajax dentro de looping
var users = [1028,885,931];
for( var i = 0; i < users.length; i+=1) {
    $.ajax({
        url: 'http://echo.jsontest.com/users/'+ users[i] ,
        dataType: "json",
        success: function( response ) {
            console.log(users[i]) //undefined
            console.log( response ); // server response
        }
    });
}
```
Isso acontece porque `i` é global dentro desse escopo, logo o valor dele é 3 e `users[i]` aparece como `undefined` pois a posição 3 não existe.


A solução é simples, devemos isolar cada índice, criando um escopo com uma função anônima auto-executável para que o valor do `i` seja preservado em cada iteração, veja o exemplo:

``` javascript função anônima auto-executável com looping
var func = [];
for( var i = 0; i < 3; i++ ){
    func[i] = (function (index) {
        return function() { 
            alert(index); 
        };
    })(i) // Índice do looping sendo passado como parâmetro
}

func[0](); // alert 0
func[1](); // alert 1
func[2](); //alert 2
```



Exemplo usando com ajax:

``` javascript função anônima auto-executável com looping
var func = [];
var users = [1028,885,931];
for( var i = 0; i < users.length; i+=1)
    $.ajax({
        url: 'http://echo.jsontest.com/users/'+ users[i] ,
        dataType: "json",
        success: (function(index) {
            return function( response ) {
                console.log(users[index]) //undefined
                console.log( response ); // resposta
            };
        })(i)// Índice do looping sendo passado como parâmetro
    });
```
O Javascript possui vários gotchas. [Jonathan Cardy](http://www.codeproject.com/Articles/182416/A-Collection-of-JavaScript-Gotchas) escreveu um post bem completo, sobre vários gotchas do Javascript, vale uma lida.

Já com relação a closures, existe um post no [stackoverflow](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work) detalhando sobre o assunto.  

