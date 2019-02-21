---
layout: post
title: "Tunando meu som"
tags: 
  - Som
  - Mod
  - DIY
---

Faz tempo que estava para restaurar e dar um _up_ em duas caixas de som das antigas que meu pai me deu. Feitas de compensado, elas já estavam esfarelando e a tinta estava bem ruim, já cheia de arranhados. E como em casa não tenho um som decente, fiquei interessado nelas para curtir minhas músicas.

O resultado final ficou bem legal. Vejam abaixo.

<iframe width="420" height="315" src="http://www.youtube.com/embed/aMsXKl-Om_w" frameborder="0" allowfullscreen></iframe>
<br/>

<!-- more -->

Neste post vou desccrever os passos que fiz para dar uma reformada nas caixas e adicionar um plus tecnológico a elas ;).

# Reforçando a estrutura

Infelizmente não tenho fotos delas antes de restaurá-las. Mas, basicamente, ela estava esfarelando e a pintura dela (preta) já estava bem ruim e grossa com a poeira..
Então o primeiro passo foi resolver as partes que estavam esfarelando. Na internet descobri que aplicar cola de madeira resolveria o problema. Quando a cola seca fica transparente, então não deixa os pontos onde são aplicadas feias estéticamente. Mas, no meu caso, como ia pintar depois, isso não foi um fator preocupante.

Dei uma lixada para remover a sujeira. Ao final, pintei de preto fosco. Usei, no total, 1,5 latas de spray.

![placeholder](/assets/images/2018-08-03-Tunando-meu-som/pintando.jpeg "Lixada e pintada")
![placeholder](/assets/images/2018-08-03-Tunando-meu-som/pintando2.jpeg "Lixada e pintada")

# Ligando o som

Eu estava apenas com as caixas de som. Ou seja, basicamente 2 caixas de madeira e dois auto falantes. Para conseguir curtir um som nelas, você precisa de uma eletrônica que reproduza áudio e um amplificador para conseguir tocar nos auto falantes.

Para dar um toque mágico de tecnologia, comprei uma placa bluetooth com amplificador. Comprei ela da china, no [aliexpress](https://pt.aliexpress.com/item/DC-8-25V-Wireless-Bluetooth-4-0-Audio-Receiver-Digital-TDA7492P-25W-25W-Amplifier-Board/32804754858.html). Liguei uma fonte de 12V nela, conectei os auto falantes e tchun! Som funcionando!

# Dando um plus

Para ficar ainda mais legal, pensei em adicionar uns leds que ficassem piscando conforme a batida da música. Afinal, porque não?

Para isso, dei uma pesquisada em várias formas e circuitos para fazer isso. No início pensava em usar arduinos para controlar o ganho da captação e controlar os leds. Mas no final, optei por um circuito analógico por ser mais simples e barato.

O circuito é bem simples. Usando um microfone de eletreto, que varia sua resistência conforme o som, o circuito abre ou fecha uma "chave" para ligar a fita de leds.

![placeholder](/assets/images/2018-08-03-Tunando-meu-som/schematic.png "Esquematico do circuito dos leds")

Depois de montado, ficou assim:

![placeholder](/assets/images/2018-08-03-Tunando-meu-som/circuito-montado.jpg "Circuito dos leds")

# Juntando as partes

Para alimentar o circuito dos leds e a placa bluetooth comprei uma fonte de 12V. Para conectar uma caixa à outra, comprei cabos. Então, dentro de uma caixa fica a fonte, a placa bluetooth e o circuito dos leds, e na outra caixa o outro circuito dos leds.

Fixei a fonte com um parafuso, e as placas com cola quente. Depois, coloquei as fitas de leds em volta da caixa. Veja as fotos abaixo.

![placeholder](/assets/images/2018-08-03-Tunando-meu-som/bluetooth-montado.jpg "Bluetooth")
![placeholder](/assets/images/2018-08-03-Tunando-meu-som/fonte-montada.jpg "Fonte de alimentação")
![placeholder](/assets/images/2018-08-03-Tunando-meu-som/projeto-montado.jpg "Projeto")
![placeholder](/assets/images/2018-08-03-Tunando-meu-som/caixa-montada.jpg "Projeto")

E Voalá! Fim.

# Melhorias/dicas/cagadas que podem ser feitas
1. Quando for usar microfones, compre todos juntos. Por que cada microfone tem um ganho e uma resistência diferente dependendo do modelo/fabricante. Um erro no meu caso foi usar diferentes nos circuitos. Nesse caso, os valores das resistências mudam.
2. Quando for colocar a placa bluetooth, você pode configurar ela. Há uma boa documentação com os comandos AT para mudar o nome e inserir uma senha (PIN) para parear.
3. Coloquei os cabos diretos nas placas. O melhor seria ter um conector no fundo da caixa. E o cabo ter os conectores nas extremidades.
4. Quando estava montando, achei que colocar uma chave geral para ligar e desligar ia ser algo desnecessário. Mas no fim, estou arrependido. O mais cômodo é ter a chave geral.

