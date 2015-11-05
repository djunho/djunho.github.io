---
layout: post
title: "Instalando o DD-WRT em um D-Link DIR300 revA"
tags: D-LINK, DIR300, DD-WRT, ddwrt, tutorial
---

Fala galera, ando meio sem tempo para postar. Ainda mais essa semana em que participarei do evento da Intel, o Intel IoT Roadshow. No próximo post comento sobre o evento.

Bom, dando vida nova a um roteador antigo aqui de casa, vou mostrar como instalar o DD-WRT nele. DD-WRT, é um firmware baseado em linux (yeaah =P) para roteadores wireless. Ou seja, é um firmware alternativo para diversos modelos de roteadores, para diversos fabricantes.

Bom, vou ser bem sucinto.

Este post, mostra como "instalar" o DD-WRT no roteador DIR300 (revA) da D-LINK. (ou seja, todo o post é baseador apenas para esse roteador, não que isso impeça alguem de se basear nele para outros fins).

E só para me garantir =P, __EU NÃO SOU RESPONSAVEL POR QUAISQUER DANOS QUE ESSE TUTORIAL POSSA CAUSAR.__ Siga as instruções a seguir por sua própria conta, risco e coragem de libertar seu roteador.

## Instalando um servidor FTP

Para começar, é necessário instalar um servidor ftp em sua maquina, a qual vai prover os arquivos binários para o roteador. Para isso vamos instalar o __tftpd-hpa__, mas pode instalar qualquer um de sua preferência.

```bash
sudo apt-get instal tftpd-hpa
```

Para configurar o servidor, modifique o arquivo: __/etc/default/tftpd-hpa__

```bash
vi /etc/default/tftpd-hpa
```

modifique a linha em que se encontra __TFT\_DIRECTORY__ para __TFTP\_DIRECTORY="/srv/tftp"__ para ficar de acordo como [FHS](http://www.pathname.com/fhs/)

```
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/srv/tftp"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="--secure --create"
```

Agora crie (se certifique) que a pasta que indicamos como diretório do servidor.

```bash
mkdir /srv/tftp
chown -R ttftp /srv/tftp
chmod -R 777 /srv/tftp
```

Os comandos para inciar, parar e verificar o status são

```bash
service tftpd-hpa start
service tftpd-hpa stop
service tftpd-hpa status
```

Pronto, servidor instalado e configurado! =D

## Instalando o DD-WRT em seu DIR300

Agora vamos instalar logo esse DD-WRT.

Baixe os arquivos para instalar.

```bash
cd /srv/tftp
wget -c http://www.dd-wrt.com/routerdb/de/download/D-Link/DIR-300/A1/ap61.ram/3581
wget -c http://www.dd-wrt.com/routerdb/de/download/D-Link/DIR-300/A1/ap61.rom/3580
wget -c http://www.dd-wrt.com/routerdb/de/download/D-Link/DIR-300/A1/linux.bin/3579
```
Note que no passo anterior, colocamos os arquivos binários no diretório _root_ do servidor ftp.


Para facilitar vamos instalar o Putty, acredite, vai facilitar a vida. =)

Vamos criar um _script bash_ para ajudar.

```bash
sudo apt-get instal putty
```

Vamos criar agora o script. Obtido do site. [oficial](http://www.dd-wrt.com/wiki/index.php/DIR300).

```bash
#!/bin/bash
echo
echo ""
echo "Enter hostname or ip address: "
read host
while true
do
   if eval "ping -c 1 $host" > /dev/null; then       
       putty telnet://$host 9000 -m redboot.txt   
       echo "Router Awake"
       break
   else
       echo "Waiting for Redboot to boot. Press CTRL + C to quit"
       sleep 1
   fi
done
```

Agora, vamos ao que interessa! _finally_! Para instalar, conecte o cabo ethernet a porta WAN do roteador.

Configure o ip da inteface de rede como __192.168.20.80__.

Desligue o retorador. Antes de ligar coloque o scrit para rodar, ao ser questionado pelo ip insira __192,168.20.81__.
Agora ligue o roteador com o botão de __reset__ pressionado, e fique com ele pressionado por uns 10 segundos.
Ao conseguir abrir uma conexão, o __putty__ vai abrir com um terminal para inserção dos comandos.

```bash
RedBoot>
```

Agora insira os seguintes comandos:

```
RedBoot> load ap61.ram
Using default protocol (TFTP)
Entry point: 0x800410bc, address range: 0x80041000-0x800680d8
RedBoot> go
```

Mesmo que pareça que travou, não se preocupe. Apenas, não desligue o roteador.

Agora, desconecte o cabo do seu roteador. Reconfigure o ip da sua interface de rede para __192.168.1.2__. Depois, conecte o cabo ethernet na porta LAN1.

Feche o Putty, e inicie novamente o script, colocando o IP __192.168.1.1__. O Puty deve abrir, com um bovo prompt

```bash
DD-WRT>
```

Agora, não desligue, nem desconecte o cabo de rede. Estes passos são muito importantes e não podem ser interrompidos.

```bash
DD-WRT> fis init
About to initialize [format] FLASH image system - continue (y/n)? y
*** Initialize FLASH Image System
... Erase from 0xbffe0000-0xbfff0000: .
... Program from 0x807f0000-0x80800000 at 0xbffe0000: .

DD-WRT> ip_address -h 192.168.1.2
IP: 192.168.1.1/255.255.255.0, Gateway: 0.0.0.0
Default server: 192.168.1.2

DD-WRT> load -r -b %{FREEMEMLO} ap61.rom
Using default protocol (TFTP)
Raw file loaded 0x80080000-0x800a8717, assumed entry at 0x80080000

DD-WRT> fis create -l 0x30000 -e 0xbfc00000 RedBoot
An image named 'RedBoot' exists - continue (y/n)? y
... Erase from 0xbfc00000-0xbfc30000: ...
... Program from 0x80080000-0x800a8718 at 0xbfc00000: ...
... Erase from 0xbffe0000-0xbfff0000: .
... Program from 0x807f0000-0x80800000 at 0xbffe0000: .

DD-WRT> reset
```

Depois disso, espere o roteador reiniciar, o que pode demorar um pouco.
Execute novamente o script colocando o IP __192.168.1.1__ e ao conectar no Putty execute os seguintes comandos. Caso não conecte de primeira, reinicie o roteador.

```bash
DD-WRT> ip_address -h 192.168.1.2
IP: 192.168.1.1/255.255.255.0, Gateway: 0.0.0.0
Default server: 192.168.1.2
DD-WRT> fis init
About to initialize [format] FLASH image system – continue (y/n)? y
*** Initialize FLASH Image System
… Erase from 0xbfc30000-0xbffe0000: …………………………………………………..
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> load -r -b 0×80041000 linux.bin
Using default protocol (TFTP)
Raw file loaded 0×80041000-0×803cffff, assumed entry at 0×80041000
DD-WRT> fis create linux
… Erase from 0xbfc30000-0xbffbf000: …………………………………………………
… Program from 0×80041000-0×803d0000 at 0xbfc30000: …………………………………………………
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> fconfig boot_script true
boot_script: Setting to true
Update RedBoot non-volatile configuration – continue (y/n)? y
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> fconfig boot_script_timeout 4
boot_script_timeout: Setting to 4
Update RedBoot non-volatile configuration – continue (y/n)? y
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> fconfig bootp false
bootp: Setting to false
Update RedBoot non-volatile configuration – continue (y/n)? y
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> fconfig
Run script at boot: true
Boot script:
.. fis load -l vmlinux.bin.l7
.. exec
Enter script, terminate with empty line
>> fis load -l linux
>> exec
>>
Boot script timeout (1000ms resolution): 4
Use BOOTP for network configuration: false
Default server IP address:
Console baud rate: 9600
GDB connection port: 9000
Force console for special debug messages: false
Network debug at boot time: false
Update RedBoot non-volatile configuration – continue (y/n)? y
… Erase from 0xbffe0000-0xbfff0000: .
… Program from 0×80ff0000-0×81000000 at 0xbffe0000: .
DD-WRT> fconfig bootp_my_ip 192.168.1.1
DD-WRT> fconfig bootp_my_ip_mask 255.255.255.0
DD-WRT> fconfig bootp_my_gateway_ip 0.0.0.0
DD-WRT> reset
```

Acredite ou não, depois disso tudo, é possível acessar a pagina de configuração do roteador. Pode ligar, desligar, tacar na parece, agora, ele já esta como o DD-WRT. Parabéns, vocÊ conseguiu.
Abra um navegador e acesse __192.168.1.1__ e tcharan! Esta lá.
