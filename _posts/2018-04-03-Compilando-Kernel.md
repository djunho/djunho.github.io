---
layout: post
title: "Compilando o Kernel"
tags: 
  - Kernel
  - Linux
  - Compilar
---

Faz tempo que queria mexer mais baixo nível no kernel do linux para entender todos os conceitos aplicados no desenvolvimento dele. Eis que então há um tempo atrás recebi um e-mail de uma reunião na Unicamp sobre a criação de um grupo de estudos na Unicamp. As reuniões estão acontecendo às terças feiras, 19 horas. Mais informações sobre o grupo acesse o [gitlab](https://gitlab.com/lkcamp) e a [página do Youtube](https://www.youtube.com/channel/UCraCE6iWUcFCJSp-vmO1D3A) onde as lives estão sendo armazenadas, e junte-se a [lista de e-mail](https://lists.libreplanetbr.org/mailman/listinfo/lkcamp), onde as informações sobre as reuniões são divulgadas.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2018-04-03-Compilando-Kernel/tux.png "Linus Torvalds em miniatura")

Vou descrever aqui, a atividade que foi realizada na primeira reunião. Compilar o Kernel. Isso mesmo, para você que achava que compilar o kernel era um bicho de sete cabeças, você está enganado. Existe toda uma magia negra para deixar esse processo menos traumático.

<!-- more -->

# Compilando o Kernel

Mãos à obra.

## Instalando as ferramentas necessárias

Algumas aplicações são necessárias para realizar todo o processo de compilação. Na prática, você nem vai ver essas aplicação sendo utilizadas, o makefile cuida disso pra você ;)

```bash
sudo apt-get install vim libncurses5-dev gcc make git exuberant-ctags libssl-dev
```

## Baixando o código do kernel

Nesse ponto, vamos criar um diretório para armazenar o código, onde iremos colocar os arquivos gerados. Estou criando uma pasta chamada "kernel" dentro de outra chamada "workspace" (mude à vontade).

```bash
mkdir -p workspace/kernel
cd workspace/kernel
```

Agora, iremos baixar todo o conteúdo dentro da pasta que acessamos acima. Pode demorar um pouco, dependendo da velocidade da sua conexão.

```bash
git clone -b staging-testing git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging.git
cd staging
```

## Drivers

O kernel é composto por diversos drivers, que podem ser compilados diretamente na imagem, ou gerados em arquivos separados como módulos (os arquivos ".ko"). Você poderia escolher drivers-a-drivers, opção-por-opção, cada detalhe, mas para facilitar esse tutorial (cujo objetivo é compilar um kernel funcional e carregar ele na máquina) vamos copiar as configurações do Kernel que está rodando em nossas máquinas.

O comando ```uname -r```, nos retorna a versão do kernel que estamos rodando atualmente. Para copiar o arquivo de configuração dessa versão, use o comando:

```bash
cp /boot/config-`uname -r`* .config 
```

Este comando vai copiar o arquivo de configuração dentro do diretório ".config".

## Alterando as configurações

Como é um kernel novo, e algumas configurações podem não estar definidas no seu arquivo de configuração, o que é normal, pois usam o valor _default_, você deve executar o comando:

```bash
make olddefconfig
```

Este comando irá te questionar sobre um monte de configurações. Para todas, você pode colocar o valor default, aceitando o que for sugerido.

Caso queira fazer alguma alteração a mais, execute ```make menuconfig```.

## Economizando tempo

O kernel é composto por diversos módulos e partes que podem nem ao menos estar sendo utilizadas na sua máquina. Afinal, nunca se sabe quando você irá conectar algum equipamento, por qualquer que seja, tanto algo antigo como um _floppy disk_, quanto algo de última geração como uma placa de vídeo externa de vários gigabytes. Assim, para economizar um tempo nessa atividade você pode "dizer" que quer compilar apenas os mesmos módulos que seu computador está usando nesse momento. Para isso execute o comando a seguir. 
**Atenção: caso execute isso, e depois ao estar utilizando o kernel que você compilou, algumas coisa podem não funcionar quando você às conectar em seu computador. Então, use apenas esse comando caso queria explicitamente essa configuração. Ao final, mostro como retirar essa versão do kernel voltando a versão que você estava utilizando anteriormente.**


```bash
make localmodconfig
```

## O grande momento

Agora o momento que você tanto sonhou.

```bash
make -j${nproc}
```

Este comando pode demorar um bocado caso não tenha executado o passo anterior. Caso contrário, vai mais rápido do que você imagina.

## Enfim compilado, e agora?

Então, é hora de colocar para rodar! Para instalar, execute o comando:

```bash
sudo make modules_install install 
```

## Configurando o bootloader (vulgo grub)

Para que o grub atualize a lista de kernels e permita que na próxima vez que reiniciar seu computador você possa escolher o kernel que você acabou de instalar, rode:

```bash
sudo update-grub2
```

Agora é só reiniciar o computador e ver se tudo deu certo.

## Deu errado?

Vixe. Nesse caso, é preciso desinstalar a imagem que você instalou. Para isso, você precisa executar os comandos abaixo.

Substitua _KERNEL-VERSION_ pela versão do kernel que você compilou e instalou. Para listar as versões de kernel instaladas rode:

```bash
dpkg --list | grep linux-image
```

Descoberta a versão, rode:

```bash
sudo rm -rf /boot/vmlinuz*KERNEL-VERSION*
sudo rm -rf /boot/initrd*KERNEL-VERSION*
sudo rm -rf /boot/System-map*KERNEL-VERSION*
sudo rm -rf /boot/config-*KERNEL-VERSION*
sudo rm -rf /lib/modules/*KERNEL-VERSION*/
```

Um exemplo para _KERNEL-VERSION_ é _4.13.0-25-generic_.

## Deu certo?

Parabéns! Agora você pode dizer que já compilou um kernel ao menos uma vez na vida.
