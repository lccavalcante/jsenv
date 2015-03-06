---
layout: post
author: Leandro Cavalcante
title: "Web Messaging - API HTML 5"
date: 2015-02-20 17:24:45 -0200
share: true
comments: true
image:
  feature: http://www.jsenv.com/post_images/A69C1ECBE1.jpg
categories: [javascript, html5]
---

_Web Messaging_ ou _Cross-document Messaging_ é uma API introduzida nas especificações da HTML 5 que permite a comunicação entre documentos de origens diferentes.

<!-- more -->

Mensagens _cross-document_ permitem que os scritps possam interagir através destes limites, proporcionando um nível de segurança não muito desenvolvido. 
Sendo assim, é essencial que o desenvolvedor cheque a origem da mensagem antes de efetuar qualquer manipulação.


A tabela abaixo, esclarece forma simples como funciona a **_Política de Mesma Origem_**:

| **URL**      				 | **Mesma origem?** | **Razão**		 |
| -------------------------- |:---------------:| -------------------:|
| http://jsenv.com/about     | Sim 			   | mesmo host, protocolo, porta  	     |
| http://jsenv.com/		     | Sim 			   | mesmo host, protocolo, porta  	  	 |
| https://jsenv.com/	     | Não 			   | protocolo diferente |
| http://jsenv.com:81 	 	 | Não 			   | porta diferente 	 |
| http://about.jsenv.com     | Não 			   | host diferente 	 |


### window.postMessage(_mensagem_, _destino_, _[portas]_)

Esta API permite que seja enviadas mensagens de texto simples, porém podemos enviar objetos transformados em String, por exemplo: **_JSON.stringify()_** para o envio e **_JSON.parse()_** no recebimento.
Confira nos exemplos abaixo:

``` javascript Conversão de um objeto em string
var obj_message = { 
	nome: 'JS Env',
	dominio: 'jsenv.com',
	atualizado: true },
	str_message = '';
/* transformando um objeto em string para envio */
str_message = JSON.stringify( obj_message );
``` 

``` javascript Conversão de uma string em objeto
/* "{"nome":"JS Env","dominio":"jsenv.com","atualizado":true}" */
str_message = JSON.parse( obj_message );
``` 

### Parâmetros:

- **mensagem**: é uma string contendo a mensagem.
- **destino**: é o endereço para onde a mensagem está sendo enviada. Ele pode adotar 3 tipos de valores:
	- uma URL absoluta: http://www.jsenv.com
	- um caractere curinga (*), para receber de qualquer destino. 
	- ou um valor que restringe o destino da mensagem (/), adotando o a política de mesma origem.
- **portas**   (opcional): 	  define um array com as portas válidas para o destino da mensagem.

As mensagens podem ser enviadas de iframes para o documento que o carrega (parent) e o processo inverso, do documento (parent) para o iframe, conforme exemplo abaixo:

``` javascript
	/* Posta mensagem para o iFrame */
	var sender = document.getElementsByTagName('iframe')[0];
	sender.contentWindow.postMessage('{ atualizar: true }', 'http://jsenv.com/');
```
``` javascript
	/* Posta a mensagem para o parent do iFrame */
	var sender = window.parent;
	sender.postMessage('{ atualizar: true }', 'http://jsenv.com/');	
```

### Escutando o evento "message"

Para receber a mensagem, basta escutarmos o evento padrão da especificação da API Web Messaging: **_message_** . Ele deve estar na página que irá manipular a mensagem. Conforme exemplo abaixo:

``` javascript
	if ( window.addEventListener ) {
		window.addEventListener( 'message', show_message, false );
	} else {
		window.attachEvent( 'message', show_message );
	}
	function show_message ( event ) { ... }
```

### O Evento de mensagem recebido possui os seguintes atributos:

- **data**: 		O conteúdo da mensagem.
- **origin**: 		A origem da mensagem.
- **source**: 		O objeto WindowProxy do destino da mensagem.
- **ports**:    	Retorna um array com as portas enviadas junto da mensagem.
- **lastEventId**: 	Retorna o identificador do último evento.

Assim, para visualizar cada atributo citado acima, a função **_show_message_** fica:

``` javascript
	function show_message ( event ) {
		if ( event.origin == 'http://jsenv.com' ) {		
			console.log( 'Mensagem 		=>', event.data );
			console.log( 'origem 		=>', event.origin );
			console.log( 'WindowProxy  	=>', event.source );
			console.log( 'Ports 			=>', event.ports );
			console.log( 'LastEventID 	=>', event.lastEventId );
		} else {
			console.log( 'Origem de envio não autorizada.' );
		}
	}
```

Referências:

- **Wikipedia:** http://en.wikipedia.org/wiki/Web_Messaging
- **Livro HTML 5 - Maujor:** http://livrohtml5.com.br/

