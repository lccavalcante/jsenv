---
layout: post
title: "Removendo Elementos de um Array"
date: 2015-03-21 19:29:00 -0300
author: Leandro Cavalcante
share: true
comments: true
image:
  feature: http://www.jsenv.com/post_images/38H.jpg
categories: [tips, javascript] 
---

Quem nunca precisou remover um elemento específico dentro de um array e deu aquela volta pra conseguir? Pois é, a ideia de postar esta dica é exatamente auxiliar na solução deste problema e fomentar a pesquisa de como certos métodos funcionam e aplica-los no nosso dia-a-dia.

## Método $.inArray da jQuery
Geralmente esta seria uma das alternativas para acharmos um algoritmo que facilitasse a solução do nosso problema, mas já pararam para pensar ou pesquisar o que exatamente o `$.inArray` faz ou como funciona?

<!--more-->

Basicamente o método `$.inArray` percorre um determinado array e retorna a posição do elemento desejado. Hmmm, isso já ajudaria bastante. Certo?

Porém, concorda que carregar a biblioteca da `jQuery` só para resolvermos isso seria desnecessário? Então vamos melhorar isso!

## Por trás do método $.inArray
Ele é semelhante ao `indexOf` nativo do Javascript e dentro da documentação da jQuery temos acesso a como o método foi escrito e podemos usar como referência.

```javascript Método descrito pela biblioteca jQuery
function inArray ( elem, array, index ) {
  return array == null ? -1 : indexOf.call( array, elem, index);
}
``` 

Porém, o `indexOf` foi adicionado como padrão no ECMA-262 em sua 5ª edição, com isso alguns navegadores antigos não suportam essa funcionalidade, por exemplo o IE8.

Então antes de escrevermos nosso método, criaremos um _Helper_ para garantirmos que esse suporte seja universal.

Primeiro verificamos se o suporte ao `indexOf` existe, caso contrário seguiremos a [recomendação de compatibilidade da MDC](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) descrita abaixo:

```javascript Helper.js
// Production steps of ECMA-262, Edition 5, 15.4.4.14
// Reference: http://es5.github.io/#x15.4.4.14
if (!Array.prototype.indexOf) {
  Array.prototype.indexOf = function(searchElement, fromIndex) {

    var k;

    // 1. Let O be the result of calling ToObject passing
    //    the this value as the argument.
    if (this == null) {
      throw new TypeError('"this" is null or not defined');
    }

    var O = Object(this);

    // 2. Let lenValue be the result of calling the Get
    //    internal method of O with the argument "length".
    // 3. Let len be ToUint32(lenValue).
    var len = O.length >>> 0;

    // 4. If len is 0, return -1.
    if (len === 0) {
      return -1;
    }

    // 5. If argument fromIndex was passed let n be
    //    ToInteger(fromIndex); else let n be 0.
    var n = +fromIndex || 0;

    if (Math.abs(n) === Infinity) {
      n = 0;
    }

    // 6. If n >= len, return -1.
    if (n >= len) {
      return -1;
    }

    // 7. If n >= 0, then Let k be n.
    // 8. Else, n<0, Let k be len - abs(n).
    //    If k is less than 0, then let k be 0.
    k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);

    // 9. Repeat, while k < len
    while (k < len) {
      // a. Let Pk be ToString(k).
      //   This is implicit for LHS operands of the in operator
      // b. Let kPresent be the result of calling the
      //    HasProperty internal method of O with argument Pk.
      //   This step can be combined with c
      // c. If kPresent is true, then
      //    i.  Let elementK be the result of calling the Get
      //        internal method of O with the argument ToString(k).
      //   ii.  Let same be the result of applying the
      //        Strict Equality Comparison Algorithm to
      //        searchElement and elementK.
      //  iii.  If same is true, return k.
      if (k in O && O[k] === searchElement) {
        return k;
      }
      k++;
    }
    return -1;
  };
}
```

Agora que podemos garantir que tudo funcione corretamente em todos os navegadores, prosseguiremos com nossa solução escrevendo nosso método para retornar a posição do elemento que desejamos remover:

``` javascript Método de sugestão
function posicaoNoArray ( elemento, array ) {
	return array == null ? -1 : array.indexOf( elemento );
}
``` 

Agora que temos nossa função para encontrar a posição do elemento dentro do _Array_, vamos resolver nosso problema.

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

	posicao_elemento_remover = posicaoNoArray( elemento_remover, elementos );
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
É muito importante utilizar como base de desenvolvimento, métodos e soluções nativas já consagradas e vastamente utilizadas. Assim, além de melhorarmos a escrita de nossos códigos, conseguimos chegar a algoritmos mais enxutos.

Existem `n` maneiras de se escrever o código acima, fique à vontade para utiliza-lo dentro do seu contexto de desenvolvimento.

Dúvidas ou sugestões, basta deixar um comentário que responderemos o mais breve possível.