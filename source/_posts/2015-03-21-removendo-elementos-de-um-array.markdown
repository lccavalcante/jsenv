---
layout: post
title: "Removendo Elementos de um Array"
date: 2015-03-21 19:29:00 -0300
author: Leandro Cavalcante
share: true
comments: true
image:
  feature: http://www.jsenv.com/post_images/C536C62904.jpg
categories: [tips, javascript] 
---

Quem nunca precisou remover um elemento específico dentro de um array e deu aquela volta pra conseguir? Pois é, a ideia de postar esta dica é exatamente auxiliar na solução deste problema e fomentar a pesquisa de como certos métodos funcionam e aplica-los no nosso dia-a-dia.

## Método $.inArray da jQuery
Geralmente esta seria uma das alternativas para acharmos um algoritmo que facilitasse a solução do nosso problema, mas já pararam para pensar ou pesquisar o que exatamente o `$.inArray` faz ou como funciona?

Basicamente o método `$.inArray` percorre um determinado array e retorna a posição do elemento desejado. Hmmm, isso já ajudaria bastante. Certo?

Porém, concorda que carregar a biblioteca da `jQuery` só para resolvermos isso seria desnecessário? Então vamos melhorar isso!

## Por trás do método $.inArray
Ele é semelhante ao `indexOf` nativo do Javascript e dentro da documentação da jQuery temos acesso a como o método foi escrito e podemos usar como referência:

``` javascript Método descrito pela jQuery
function inArray ( elem, array, index ) {
	return array == null ? -1 : indexOf.call( array, elem, index);
}
``` 

ou então

``` javascript Método de sugestão
function inArray ( elem, array ) {
	return array == null ? -1 : array.indexOf( elem );
}
``` 

Agora que temos nossa função para encontrar a posição do elemento dentro do Array, vamos resolver nosso problema.

Dado um array:

``` javascript
var elementos = [10, 20, 30, 40, 50];
```

Vamos encontrar a posição do elemento cujo valor seja igual a `30` e guardar em uma variável, deixaremos também, uma variável para guardar o elemento removido.

``` javascript
var elementos 					= [10, 20, 30, 40, 50],
	elemento_remover			= 30,
	elemento_removido,
	posicao_elemento_remover;

	posicao_elemento_remover = inArray( elemento_remover, elementos );
```

Nossa variável `posicao_elemento_remover` será igual a `2` lembrando que o índice do array inicia-se em `0`.

Agora, usaremos o método `splice` do `Array` para remover o elemento:

``` javascript
elemento_removido = elementos.splice( posicao_elemento_remover, 1);
```

Seu primeiro parâmetro é a posição onde iniciará a remoção e o segundo parâmetro a quantidade de elementos que serão removidos. Como queremos somente remover um elemento, o valor adotado é `1`.

Após essa etapa, temos:

- `elementos` 		  = `[10, 20, 40, 50]`.
- `elemento_removido` = `[30]`.

## Código completo

``` javascript
var elementos 					= [10, 20, 30, 40, 50],
	elemento_remover			= 30,
	elemento_removido,
	posicao_elemento_remover;
	//
	function posicaoNoArray ( elemento, array ) {
		return array == null ? -1 : array.indexOf( elemento );
	}
	//
	posicao_elemento_remover = posicaoNoArray( elemento_remover, elementos );
	//
	elemento_removido = elementos.splice( posicao_elemento_remover, 1);
	//
	console.group( 'Resultado:' );
	console.log(' Array final:', elementos);
	console.log(' Elemento removido: ', elemento_removido);
	console.groupEnd();
```

## Conclusão
É muito importante utilizar como base métodos e soluções adotadas por bibliotecas já consagradas e vastamente utilizadas. Assim, além de melhorarmos a escrita de nossos códigos, conseguimos chegar a algoritmos mais enxutos.

Existem `n` maneiras de se escrever o código acima, fique à vontade para utiliza-lo dentro do seu contexto de desenvolvimento.

Dúvidas ou sugestões, basta deixar um comentário que responderemos o mais breve possível.