---
layout: post
title: "Convertendo timestamp para Data Hora no LibreOffice"
tags: 
  - LibreOffice
  - timestamp
  - spreedsheet
---

Essa semana estava precisando analisar os dados de um equipamento. Os dados eram compostos por um timestamp (UNIX específicamente), e dados de alguns sensores.

Exportei tudo para um [arquivo CSV](https://pt.wikipedia.org/wiki/Comma-separated_values). E então me deparei que observar o dado do UNIX timestamp da maneira cru é algo não muito intuítivo. Então fiquei imaginando como poderia transformar aquele número gigante em algo mais natural.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2018-09-29-timestamp-para-datahora-no-libreoffice/ts-para-datahora.png "Conversão")

<!-- more -->

Depois de pesquisar um pouco, encontrei como fazer isso. Descrevi abaixo a fórmula  e o motivo.

# Informação de Data Hora

Em UNIX timestamp, é um contador de segundos que começa na data de **01/01/1970 à 00:00:00**. Exemplo, o número 1 representa "01/01/1970 00:00:01".

O timestamp usado pelo LibreOffice, segundo algumas informações encontradas no [forums](https://forum.openoffice.org/en/forum/viewtopic.php?f=13&t=606) e na sua configuração, o valor default começa em **30/12/1899 às 00:00:00**. E é representado de forma que a parte inteira é o número de dias, e a parte que vem depois da vírgula representa o horário. Exemplo, o número ```1,25``` representa "31/12/1899 06:00:00".

# A conversão

Como existe essa diferença entre o dia 0 de cada uma dessas representações de data hora, o timestamp do UNIX está ```25569``` (dias) a frente.

Um dia possui ```86400``` segundos. Então para realizar a conversão podemos simplesmente usar uma fórmula pegando o UNIX timestamp de outra célula. Veja abaixo.

```
=(A2/86400)+25569
```

Sendo que a célula A2 contém o valor do UNIX timestamp. Note que, caso mude o valor default do dia 0 do timestamp do LibreOffice a fórmula deve ser alterada.

# Visualizando

Para visualizar o dado no formato de Data Hora, basta formatar a célula em que foi realizada a converção para Data Hora ou da forma que lhe for melhor.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2018-09-29-timestamp-para-datahora-no-libreoffice/visualizacao.png "Visualização")


# Configuração

Nas configurações do LibreOffice tem como alterar o dia 0 do timestamp dele.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2018-09-29-timestamp-para-datahora-no-libreoffice/configuracao.png "Configuração")

