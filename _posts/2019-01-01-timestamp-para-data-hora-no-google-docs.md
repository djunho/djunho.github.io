---
layout: post
title: "Convertendo timestamp para Data Hora no Google Docs"
tags: 
  - Google
  - Docs
  - timestamp
  - spreedsheet
---

Da mesma forma como no [post anterior](http://danieljunho.com/2018/09/29/timestamp-para-data-hora-no-libreoffice.html), onde converti unix timestamp para um formato tradicional de data hora, precisei precisei fazer o mesmo com o google docs. E descobri que funciona da igual ao libre office.

<!-- more -->

No caso do Google docs o dia 0 é o dia 30/12/1899 00:00:00. E é representado de forma que a parte inteira é o número de dias, e a parte que vem depois da vírgula representa o horário. Exemplo, o número ```1,25``` representa "31/12/1899 06:00:00".

A formula então fica da seguinte maneira.
```
=(A2/86400)+25569
```

Ou ainda,
```
=(A2/86400) + DATA(1970;1;1)
```

Depois disso, só mudar a formatação para data hora para visualizar os dados com o formato mais "humano".

