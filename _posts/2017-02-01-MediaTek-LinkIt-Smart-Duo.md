---
layout: post
title: "MediaTek LinkIt Smart 7688 - Review"
tags: MediaTek, LinkIt, 7688, Arduino, OpenWrt, wifi, review, revisão, Atmel
---

E o ano começou agitado. Agora tenho em mãos a [MediaTek LinkIt™ Smart 7688](https://labs.mediatek.com/en/platform/linkit-smart-7688), uma beleza!

Este "brinquedo" possui um processador MT7688AN ([datasheet](https://labs.mediatek.com/en/download/50WkbgbH)) da mediaTek, rodando a 580MHz com 32MB de Flash e 128MB de memória RAM (DDR2) com o OpenWrt, uma distribuição linux voltada para roteadores. Mas além disso, existe a versão "Duo" que contempla um micro ATmega32U4, para aplicações simultâneas usando a própria IDE do arduino. Ambos os micros conectados e comunicando via uma porta serial, deixando tudo mais fluido e prático possível.

Seu único porém, que eu identifiquei até agora, é a falta de bluetooth.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-02-01-MediaTek-LinkIt-Smart-Duo/mediatek-linkit-smart-duo.jpg "A placa!")

<!-- more -->

O fato de ter dois processadores na mesma placa permite aplicações bem interessantes, assim como um _core_ heterogêneo, mas nesse caso, fisicamente separados. Um exemplo prático seriam aplicações onde o ATmega controla e atua, deixando o processamento e tomada de decisões para o micro da mediaTek (MT7688AN), que roda um linux. Um processador com linux simplifica muito as aplicações, pois deixa elas mais flexíveis para usar diversas plataformas e frameworks prontos e mais alto níveis, além de resolver todos os problemas de conectividade com a rede (IoT easy-easy).

A versão [Duo](https://www.seeedstudio.com/LinkIt-Smart-7688-Duo-p-2574.html) custa $15,90, e a versão [normal](https://www.seeedstudio.com/LinkIt-Smart-7688-p-2573.html) custa $12,90 (valores conferidos em 01/03/2017). Pois é, como muitos já sabem, os micros da Atmel de AVR atualmente são bem [caros](http://www.digikey.com/products/en/integrated-circuits-ics/embedded-microcontrollers/685?k=ATmega32U4), principalmente o ATmega, o que justifica bem essa diferença de preço.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-02-01-MediaTek-LinkIt-Smart-Duo/linkit7688_detalhes.jpg "Visão geral sobre a placa.")

Ele já vem com uma GUI pronta, para quando se conectar já configurá-lo. Então quando começar, conecte-se na rede que ele cria e configure-o usando o próprio navegador em [http://mylinkit.local](http://mylinkit.local). A partir daí, é correr para o abraço. Use as ferramentas padrões para acessá-lo, como ssh, ou via serial e pronto, tem acesso a usar tudo que precisa.

Veja o mapa de pinos da versão [Duo](https://labs.mediatek.com/en/download/GHkUS0qj). Para ver a versão normal, [clique aqui](https://labs.mediatek.com/en/download/189LRncF).

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-02-01-MediaTek-LinkIt-Smart-Duo/duo-pinout.jpg "Mapa de pinos da versão \"duo\".")

A própria página da [mediatek](https://docs.labs.mediatek.com/resource/linkit-smart-7688/en) tem uns tutoriais para você aproveitar com bastante documentação, que vem crescendo a cada dia, e também existe um [forum](https://forum.labs.mediatek.com/en/) para dúvidas.

Agora é só brincar!


