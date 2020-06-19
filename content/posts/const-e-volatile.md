---
categories: [ "code" ]
date: "2010-06-04"
tags: [ "draft",  ]
title: "Const e Volatile"
---
Padrão C (ISO/IEC 9899:1990)


Chamamos de qualificador de tipo as palavrinhas mágicas const e volatile. Na prática elas definem como uma determinada variável será usada e se comportará durante a vida do programa.

Uma variável const não pode ser alterada pelo programa durante sua execução, apenas durante sua inicialização:


No exemplo acima, o valor de pi não pode mais ser alterado. Só que repare que ele foi, em determinado momento, alterado com um valor constante: na sua inicialização. Isso quer dizer que pi:

 - é uma variável no programa representada por um local na memória endereçável pelo programa;
 - não é um define do pré-processador que irá virar uma constante literal (3.14, por exemplo).

Teoricamente a região da memória que contiver uma variável const pode ser qualificada pelo sistema operacional como somente-leitura, mas isso não é uma obrigação. É obrigação do compilador avisar sobre tentativas de alteração da variável no meio do programa, mas nem sempre é possível enxergar que a memória não é alterável. Dessa forma, resultados imprevisíveis podem ocorrer.

Eu costumo usar variáveis const no lugar de defines. Além de ganhar na tipagem as constantes não precisam ser necessariamente globais, nem acessíveis por outros módulos. Um outro uso muito comum é criar variáveis locais que você sabe que não devem ser alteráveis por ninguém, como o tamanho de matrizes primitivas.


O significado do volatile teoricamente muda de implementação para implementação, mas na prática é uma forma de definir uma variável que está sendo acessada por outros programas/threads/entidades espíritas que podem alterar o seu valor sem seu programa notar quando.

O exemplo clássico da API Win32 é o InterlockedIncrement, que realiza operações atômicas em valores inteiros. Para fazer isso é necessário usar um recurso interno disponível pelo processador que irá modificar a memória sem intrusão de outras threads/processadores.

Variáveis volatile geralmente interagem de alguma forma com o sistema em que rodam, e são representadas por ponteiros para memória retornada por esse sistema ou documentada como sendo de uso específico.

É possível que exista uma variável que não pode ser modificada pelo seu programa, mas é modificada pelo sistema, de forma que ela é uma mutante!


A definição de gsystemClock é de uma memória que não pode ser alterada; só que ela é, pelo sistema. Então a variável também é volatile. No entanto, independente de ser const ou volatile, o tipo nunca será alterado, apenas qualificado. São duas coisas diferentes na linguagem.