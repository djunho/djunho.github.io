---
layout: post
title: "Problema de acentos no Sublime Text"
tags: 
  - Sublime Text
---

Esta semana me deparei com um pequeno problema. O meu editor de texto, <s>(o incrível)</s> Sublime Text parou de escrever acentos, deixando **ã**, **é**, e **ó** como **~a**, **'e** e **'o**.
Utilizo o Ubuntu 14.04 e após alguma pesquisa este problema pode ser resolvido reconfigurando o ibus, desabilitando-o.

<!-- more -->

Execute o comando:

```bash
im-config
```

Ao ser questinado **"Você quer selecionar explicitamente a configuração de usuário?"*** selecione **Sim**.
Após, selecione a opção **none**, clique em OK e reinicie o computador.

Esta foi a maneira que eliminei este problema grosseiro e inoportuno. Teclados, sempre um problema para a humanidade.
