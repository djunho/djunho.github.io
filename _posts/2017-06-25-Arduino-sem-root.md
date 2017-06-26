---
layout: post
title: "Usando a IDE do Arduino sem root"
tags: arduino, IDE, root
---

Já faz um tempo que toda vez que tenho que usar a IDE do arduino tenho que rodá-la como _root_ e isso tem me incomodado um pouco. Isso porque a IDE realiza acesso as portas seriais para carregar as aplicações nas placas. Mas se eu rodar a IDE como _root_ onde está a segurança nisso? Qualquer coisa pode acontecer com um sistema quando rodar um programa como _root_.

Então, procurei uma forma de solucionar esse problema. Praticamente, todas as pessoas que encontrei na internet resolveram o problema da seguinte maneira. Adicionando o usuário que utilizam no grupo de usuários que controlam o dispositivo de conexão com o Arduino.

```
sudo usermod -a -G dialout $USER

```

Mas na minha máquina não funcionou. Então vamos entender o problema e encontrar a solução. 

<!-- more -->

Dei uma olhada e percebi que no meu sistema não existe o usuário "dialout". Para identificar o grupo de usuários que você deve se adicionar use o comando __ls__ sobre os dispositivos que vai acessar, ou seja as portas seriais (pasta /dev), e veja quais grupos tem acessos a esses dispositivos.

```
$ ls -l /dev/tty*
...
crw-rw---- 1 root   uucp 188,  0 jun 25 20:48 /dev/ttyUSB0
...

```
A quarta coluna no resultado do comando é o grupo de usuários com acesso a esse dspositivo. Normalmente as placas de arduino ficam sempre nos dispositivos __/dev/ttyUSB!*__ ou __/dev/ttyACM!*__. 

Assim, insira o grupo correto no comando, reinicie o login para ter efeito e seja feliz.

```
sudo usermod -a -G uucp $USER
```

Agora sim, conciência tranquila ao usar a IDE =).

