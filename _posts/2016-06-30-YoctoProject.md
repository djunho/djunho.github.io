---
layout: post
title: "Yocto Project"
tags: Linux, embarcados, yocto
---

Recentemente comecei a trabalhar em um projeto com linux embarcado, e diante da necessidade de melhorar o consumo, foi necessário tentar otimizar o linux que rodava, para tentar reduzir todos os processos e todas os programas e aplicaçãoes que rodam desnecessariamente nesse projeto consumindo processador e por sua vez, consumo de energia.

Enfim, o que eu precisava era construir uma distribuição linux customizada para uma aplicação embarcada dedicada. E assim, surgiu o momento para me dedicar para aprender um pouco mais do [Yocto Project](https://www.yoctoproject.org/).

<!-- more -->

De maneira muito sucinta, o Yocto Project é um conjunto de ferramentas para auxiliar na criação de imagens customizadas de sistemas baseados em Linux para sistemas embarcados. Baseada no [Poky](http://www.pokylinux.org/), que por sua vez foi baseada no [OpenEmbadded](http://www.openembedded.org/wiki/Main_Page).

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2016-06-30-YoctoProject/yocto-environment.png "Arquitetura do Yocto Project")

O sistema funciona processando instruções presentes em metadata atráves do BitBake, gerando então as imagens, e SDK's, entre outras coisas, para o Sistema final. Então, os metadata são um conjunto de receitas que descrevem a construção de cada componente do sistema e o bitbake executa essas receitas. E sim, existem várias receitas prontas, como Qt, banco de dados, Chromium, Java, Phyton, Perl, Ruby, Lua, Node.js, e por ai vai. Simples como mágica.

Ainda estou tentando entender todas as variáveis e o funcionamento desse sistema, mas existem diversos materiais e tutoriais disponíveis na internet, como a [propria documentação](https://www.yoctoproject.org/documentation) e em protuguês o [portal embarcados](www.embarcados.com.br).

Bora aprender algo novo!
