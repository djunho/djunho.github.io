---
layout: post
title: "A brincadeira do IoT"
tags: IoT, ESP, ESP8266
---

Já faz um tempo que ando brincado com IoT, especialmente o módulo ESP8266 da Espressif. Realmente é incrivel o que esses chineses são capazes de fazer hein. Com um pequeno chip, eles conseguiram imbutir **toda** uma inteface simples para uso do Wifi (802.11) e pode ser encontrado por "apenas" [U$ 5](http://pt.aliexpress.com/wholesale?catId=0&initiative_id=SB_20150919053814&SearchText=ESP8266+01 "No momento em que escrevo esse artigo"), algo simplesmente inovador!

<p align="center">
  [placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/ESP8266-01.jpg "Módulo modelo 01")
</p>

Depois de um tempo com o chip no mercado eles lançaram o SDK, o que revolucionou ainda mais aos hobbistas de plantão, ampliando muito as possibilidades de aplicações.
Pois antes, os módulos vinham com um firmware interno de comandos AT, algo que, já era notável, simplesmente pelo tamanho, mas agora, eles possibilitaram a criação de firmwares customizaveis. O que significa que todo o processamento das suas aplicações pode ser feito pelo ESP, não sendo mais necessário o uso de um processador externo para interagir com ele (por exemplo um arduino). E já existem diversos módulos diferentes, variando antenas e numeros de GPIO's disponíveis.

Lógico que em pouco tempo já surgiram muitos firmwares customizados pela comunicade, um deles é o [NodeMCU](http://nodemcu.com/index_en.html "NodeMCU"). A grosso modo, é um firmware que provê ao usuário uma maquina lua para rodar seus softwares, já provendo toda uma API de uso. O que facilita muito seu uso e prototipação. E com uma [documentação](https://github.com/nodemcu/nodemcu-firmware "Git Hub") bem "completa", mas meio mal escrita as vezes.
Existe também, uma [IDE do arduino](https://github.com/esp8266/Arduino) para módulo, o que é algo muito legal para o pessoal que já esta acostumado com essa interface, carregando seu programa, que na verdade é um firmware customizado, a cada vez que faz que carrega seu programa arduino. Abordagem interessante para algumas aplicação, pois não tem o *overhead* de uma máquina lua, como é o caso do NodeMCU.

Isso é um prato cheio para muitos *makers* de plantão. Vamos ver os projetos que podem sair com essa belezura pelo mundo.
