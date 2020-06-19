---
categories: [ "code" ]
date: "2018-05-22"
tags: [ "draft",  ]
title: "SSL e seu limite de pacote"
---
O protocolo TLS/SSL tem por objetivo criar uma camada de criptografia assimétrica para a aplicação. E quando eu falo em camada não estou me referindo às camadas OSI. Nem às camadas TCP/IP. Isso porque o SSL não se encaixa em nenhuma das duas. Ele interfere com muitas, inclusive a aplicação. E aprendi isso a duras penas: na ponta do depurador.

O pacote SSL tem um limite de 16 KB, ou 16384 bytes. Esse é o limite que será respeitado por qualquer implementação do protocolo, o que inclui o uso de Boost.Asio e seu uso da OpenSSL. O que isso quer dizer na teoria é que você não pode trafegar sentido server=>client nada maior que 16k bytes. O que isso quer dizer na prática é que sua aplicação não pode escrever mais que 16k bytes de uma vez no socket que vai dar pau.

Sim, a camada de aplicação tem que estar ligada que existe SSL abaixo dela.

Isso quer dizer que este snippet de código, por exemplo:


Não é inocente e não funciona sempre. Se sock for um socket cuja comunicação está encriptada por SSL (em outras palavras -- em Boostês -- ele for um sslsocket) você precisa escrever output em pequenas quantidades. Como em outra implementação inocente:


Se isso não for feito e a ponta server escrever, digamos, 512KB, ou 17KB, ou qualquer coisa acima de 16KB, ela irá receber... 16 KB. E acabou. O resto se perder.

Portanto, quando for mexer com SSL, esqueça OSI e esqueça TCP/IP. As coisas funcionam de uma maneira muito mais esotérica que qualquer programador de redes jamais viu, e jamais verá.