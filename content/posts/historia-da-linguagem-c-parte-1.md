---
categories:
- code
date: '2007-08-01'
tags:
- ccpp
title: 'História da Linguagem C: Parte 1'
---

Confesso que adoro estudar sobre a história da linguagem C. Essa verdadeira adoração pela linguagem me fez estudar suas precursoras, como as linguagens BCPL e B. Posso dizer que todo esse conhecimento, no final das contas, valeu a pena. Hoje entendo muito melhor as decisões tomadas na criação da linguagem e, principalmente, a origem de algumas idiossincrasias e boas idéias que permaneceram até hoje.

![Esse distinto cavalheiro inglês é Martin Richards](/img/martin_richards.gif)

Em 21 de julho de 1967 Martin Richards libera [o manual] da sua recém-criada linguagem BCLP. Na verdade, ela havia sido criada em 66 e implementada na primavera do ano seguinte no Instituto de Tecnologia de Massachusetts (vulgo MIT). Seus objetivos eram claros, como para todo criador de uma nova linguagem: melhorar uma linguagem anterior. Nesse caso, foi uma melhoria da Combined Programming Language (CPL), retirando, de acordo com Martin, "todas aquelas características da linguagem completa que tornavam a compilação difícil".

E BCPL era de fato bem simples. Não tinha tipos, era limpa e poderosa. Porém, mais importante que tudo isso, ela era portável. E essa portabilidade, aliada ao fato que escrever compiladores para ela era bem mais simples (alguns compiladores rodavam com apenas 16 KB), a tornaram especialmente popular na época.

Essa portabilidade era obtida com o uso de um artifício mais ou menos conhecido da comunidade C/C++ hoje em dia: a divisão entre código objeto e código final. O compilador era dividido em duas partes: a primeira parte era responsável por criar um código em estado intermediário feito para rodar em uma máquina virtual. Esse código era chamado de O-code (O de object). A segunda parte do compilador era responsável por traduzir esse O-code no código da máquina-alvo (onde iria ser rodado o programa). Essa sacada genial de 40 anos atrás permitiu que fosse mais simples fazer um compilador para uma nova plataforma e portar todo o código que já tinha sido escrito para uma plataforma anterior, driblando o grande problema daquela época: a incompatibilidade entre plataformas.

![Processo de geração do BCPL O-code](/img/ocode.gif)

Perceba que é possível fazer toda a parte do compilador detrás do código-objeto uma única vez e, conforme a necessidade, criar novos interpretadores BCPL para máquinas diferentes.

![Interpretação do o-code para código da máquina alvo](/img/portable_bcpl.gif)

O código intermediário é gerado para uma máquina virtual. O interpretador, cerca de um quinto do compilador, tem a função de traduzir o código gerado para a máquina-alvo. Qualquer semelhança com Java ou .NET não é mera coincidência. Pois é. As boas idéias têm mais idade que seus criadores.

É inevitável também não fazer a associação entre essa forma de funcionamento do compilador BCPL e a divisão feita em C/C++ entre o pré-processador, o compilador e o ligador (linker, em inglês).

![Processo de compilação C/C++](/img/ccpp_build_steps.gif)

O uso do pré-processador na linguagem C facilitou a portabilidade por um bom tempo, quando não existiam typedefs. Diferente do BCPL, C já tinha tipagem, o que quer dizer que era necessário escolher o espaço de armazenamento que seria utilizado para as variáveis. Com o pré-processamento, essa escolha pode ser feita de maneira seletiva, documentada e generalizada.

    #ifdef SBRUBLE_PLATFORM
    #define UINT unsigned char /* space limitation */
    #else
    #define UINT unsigned int
    #endif

Como é natural, o código-fonte de uma aplicação tende a crescer em muitas linhas durante sua evolução, especialmente se estamos falando de sistemas operacionais. A compilação desse código vai tomar cada vez mais tempo no processo de desenvolvimento. Por isso, manter esse código-fonte em um mesmo arquivo eventualmente torna-se inviável, tornando a compilação de módulos separados uma solução pra lá de elegante. Compila-se apenas o módulo que foi modificado e liga-se esse módulo com módulos pré-compilados.

Para continuar lendo sobre a história da linguagem existe uma [segunda parte].

[o manual]: /doc/bcpl.pdf
[segunda parte]: /historia-da-linguagem-c-parte-2