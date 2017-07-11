---
layout: post
title: "Conectando o ESP8266 no IBM Bluemix IoT"
tags: arduino, Embarcados, Bluemix, IoT, ESP8266
---

Hoje saiu meu [primeiro artigo](https://www.embarcados.com.br/conectando-o-esp8266-no-bluemix-iot/) no [portal Embarcados](https://www.embarcados.com.br/). Que fantástico *_*

Um artigo bem simples, mostrando um passo a passo para os iniciantes de como conectar o ESP8266 (ou qualquer outra placa se você abstrair) na plataforma da IBM, o Bluemix. Coloco-o aqui na integra. O original se encontra [aqui](https://www.embarcados.com.br/conectando-o-esp8266-no-bluemix-iot/).

<!-- more -->

---

# Conectando o ESP8266 no Bluemix IoT

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/inicial.png "IBM Bluemix com o ESP8266")

Muitos já sabem que é possível usar o ESP8266 como um microcontrolador, utilizando-o no lugar do Arduino. Com essa possibilidade fica muito mais fácil realizar diversos projetos de maneira mais simples. Um ESP8266 conectado direto ao sensor e enviando dados para a nuvem, facilita muito os projetos mais simples que tentam abocanhar um pouco dessa grande área que é a internet das coisas (IoT).

Contudo, ainda vejo muitas pessoas, nos hackathons que participei, com grandes dificuldades para conseguir realizar a conexão do ESP8266 com algumas plataformas de IoT. Neste artigo vou mostrar como realizar essa conexão com a plataforma da IBM, o Bluemix.

Pré-requisitos
----------------

* Uma conta no Bluemix;
* Um ESP8266;
* Um computador com o Arduino IDE.
 
Observações sobre os materiais utilizados
--------------------------------------------

* Arduino IDE versão 1.8.3 (Windows e Linux);
* Configuração no Boards Manager do ESP8266 versão 2.3.0;
* Biblioteca PubSubClient versão 2.6.0;
* ESP8266 12E;
* Windows 10 Profissional.

Configurando a IDE Arduino para programar o ESP8266
------------------------------------------------------

Para configurar a IDE para programar o módulo ESP8266, execute os seguintes passos:

* Dentro da interface da IDE clique em File > Preferences;
* Adicione a URL *'http://arduino.esp8266.com/stable/package_esp8266com_index.json'* no campo Additional Boards Manager URLs e clique OK;
* Clique em "Tools > Board > Bards Manager";
* Encontre a placa esp8266 by ESP8266 Community, selecione a versão 2.3.0 e clique em instalar.

Assim teremos disponíveis as placas da linha ESP8266, suas bibliotecas e exemplos incluídos.
 
Configurando o IBM Bluemix
----------------------------

Primeiro, é preciso criar e configurar uma aplicação na plataforma bluemix para receber os dados do nosso dispositivo. Para isso, acesse o portal do [bluemix](https://www.ibm.com/cloud-computing/bluemix/), e crie uma conta. Depois de criada, realize o login. Crie uma organização e um space para suas aplicações. A região escolhida no meu caso foi “US South”.

Você irá se deparar com uma página similar a apresentada na Figura 1.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/1.png "Dashboard da plataforma Bluemix")
Figura 1 - Dashboard da plataforma Bluemix

Dentro da categoria “Apps”, busque por “Internet of Things Platform Starter”. Criaremos uma instância desse aplicativo na cloud.

Dê um nome a nosso aplicativo, e finalize a criação do App clicando em “Create”. Esta aplicação nos criará uma interface com Node-Red, a qual utilizaremos para receber e tratar os dados dos sensores recebidos. No meu caso, dei o nome de “ESP8266-IoT-Embarcados”.
 
![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/2.png "Criando uma aplicação (Internet of Things Platform Starter) no Bluemix")
Figura 2 - Criando uma aplicação no Bluemix

Após isso, a aplicação será inicializada. O processo de inicialização da aplicação pode levar alguns minutos.


![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/3.png "Iniciando o serviço na plataforma.")
Figura 3 - Iniciando o serviço na plataforma.

Após isso, acesse a página criada para sua aplicação. No meu caso [este link](https://esp8266-iot-embarcados.mybluemix.net/). Esta página é para a criação de sua aplicação Node-Red. Ao acessá-la pela primeira vez, você terá que configurá-la com um usuário e senha, fato que fortemente recomendo, pois a página é pública e aberta a toda a internet. Depois disso, você poderá acessar o editor e criar sua aplicação. Um pequeno exemplo é criado quando começamos, mostrado na Figura 4.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/4.png "Aplicação NodeRed")
Figura 4 - Aplicação NodeRed
 
Aplicação embarcada
--------------------

Não vou entrar no mérito da aplicação embarcada. Mas deixo aqui todo o código fonte usado.

Basicamente, a conexão com a cloud é feita com [MQTT](https://www.embarcados.com.br/mqtt-protocolos-para-iot/), um protocolo já vastamente discutido e cheio de exemplos aqui no Embarcados.

Para o funcionamento do código é necessário adicionar a biblioteca “PubSubClient” em Library Manager.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/5.png "Library Manager do Arduino")
Figura 5 - Library Manager do Arduino

Segue o código. Modifique as variáveis necessárias para conectar-se na rede Wifi, e para conectar-se na cloud.
 
```C	
//__ Aplicação de exemplo do artigo do embarcados
// Conecta-se a uma rede wifi, conecta-se na cloud da IBM via MQTT e envia dados
//
// Daniel Junho - 09/07/2017
 
#include <ESP8266WiFi.h>
#include <PubSubClient.h>
 
//__ Informações do WIFI
const char* ssid =     "SSID";
const char* password = "Senha Wifi";
 
//__ Informações do dispositivo
#define DEVICE_TYPE  "Nos_da_rede"
#define DEVICE_ID    "Dispositivo1"
 
//__ Informações da conexão com o servidor
#define ORG     "xxxxxx"
#define TOKEN   "xxxxxxxxxxxxxxxxxx"
 
//__ Variáveis de conexão com o servidor (Não customizaveis)
char server[]   = ORG ".messaging.internetofthings.ibmcloud.com";
char topic[]    = "iot-2/evt/status/fmt/json";
char authMeth[] = "use-token-auth";
char token[]    = TOKEN;
char clientId[] = "d:" ORG ":" DEVICE_TYPE ":" DEVICE_ID;
 
WiFiClient wifiClient;
PubSubClient client(server, 1883, NULL, wifiClient);
 
//__ Função de setup do arduino
void setup() {
  //__ Inicializa a serial
  Serial.begin(115200);
  Serial.println();
  Serial.print("Conectando-se na rede "); Serial.print(ssid);
 
  //__ Conecta-se na rede WIFI
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  } 
  Serial.println("");
 
  Serial.print("Conectado, endereço de IP: ");
  Serial.println(WiFi.localIP());
}
 
//__ Envia os dados para a cloud
void enviaDado(String nome_campo, int dado){
  //__ Formata a string que será envia para a cloud (JSON)
  String payload = "{\"d\":{\"" + nome_campo + "\":";
  payload += dado;
  payload += "}}";
 
  Serial.print("Sending payload: ");
  Serial.println(payload);
 
  //__ Envia o dado
  if (client.publish(topic, (char*) payload.c_str())) {
    Serial.println("Publish ok");
  } else {
    Serial.println("Publish failed");
  }
}
 
//__ Variável a ser publicada
int counter = 0;
 
//__ Função principal
void loop() {
  
  //__ Verifica se está conectada a cloud para envio dos dados
  if (!!!client.connected()) {
   //__ Caso não esteja conectada, tenta a conexão
    Serial.print("Reconectando-se em ");
    Serial.println(server);
    while (!!!client.connect(clientId, authMeth, token)) {
      Serial.print(".");
      delay(500);
    }
    Serial.println();
  }
 
  //__ Envia um dado para a cloud
  enviaDado(String("counter"), counter);
  
  //__ Incrementa o contador, mudando o valor a ser enviado para a cloud.
  ++counter;
 
  //__ Faz o envio a cada 10 segundos.
  delay(10000);
}
```
 
Enviando os dados para a Cloud
-------------------------------

Existem duas maneiras de receber seus dados na Cloud. Em uma das maneiras os dados são públicos e podem ser acessados por qualquer pessoa, desde que saiba o device id do dispositivo. Na outra maneira, os dados são privados, e a única forma de acesso é com credenciais.
 
Envio dos dados de modo público
---------------------------------

A vantagem do envio dos dados em forma pública é que existe uma página para verificar se o seu dispositivo está enviando dados corretamente. Dessa forma, conseguimos validar uma das pontas do nosso sistema, do dispositivo para a nuvem.

Para isso, modifique o _define ORG_ para __quickstart__ na aplicação do ESP8266. Isso implica que os dados publicados serão enviados para uma organização já conhecida e pública da plataforma. Note que os valores do _define TOKEN_ e o método de autenticação não serão validados na cloud, e portanto, seu valor não faz diferença na aplicação.

Depois de carregada sua aplicação no ESP8266, verifique se o mesmo se conectou na sua rede WIFI, observando os dados sendo impressos pela serial. Assim que conectado, o dispositivo passará a enviar os dados para a nuvem. Para validar os dados que chegam na cloud acesse [esta página](https://quickstart.internetofthings.ibmcloud.com/) e insira o nome que o dispositivo se registra na cloud, nome definido no *define DEVICE_ID*. Os dados recebidos então serão apresentados na página.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/6.png "Página de quickstart de dispositivos conectados")
Figura 6 - Página de quickstart de dispositivos conectados

Envio dos dados de modo privado
--------------------------------

Depois de validada a conexão entre o _device_ e a _cloud_, vamos migrar os dados do nosso dispositivo para um envio de dados seguro e criptografado.

Para isso, acesse o serviço de internet das coisas, um dos serviços criados juntos com nossa aplicação.
  
![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/7.png "Serviços rodando na plataforma.")
Figura 7 - Serviços rodando na plataforma.

Depois entre na dashboard para adicionar e gerenciar os dispositivos.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/8.png "Dashboard de dispositivos")
Figura 8 - Dashboard de dispositivos

Dentro dessa dashboard, selecione no menu a opção dispositivos, e depois clique no botão “Incluir dispositivo”.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/9.png "Cadastro de dispositivos.")
Figura 9 - Cadastro de dispositivos.

Como não existe nenhum tipo de dispositivo ainda, teremos que criar um tipo para a nossa rede. Dessa forma, clique “Criar tipo de dispositivo”, depois clique novamente em “Criar tipo de dispositivo”, e dê um nome a ele.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/10.png "Cadastro de tipo de dispositivo")
Figura 10 - Cadastro de tipo de dispositivo

Depois, uma página onde serão apresentados alguns atributos para vincular ao tipo de dispositivo aparecerá. Essas opções são opcionais, adicione-as conforme for o contexto da sua aplicação.

Depois de criado o tipo, iremos finalizar a criação do dispositivo. Dê um nome/ID a ele, e finalize a criação.

No final da criação, seremos questionados sobre o token de autenticação. Sugiro deixar o serviço criar um automaticamente para você. Mas, caso queira, existe a opção de fornecer o token.

Ao final da criação do dispositivo teremos um resumo, onde as informações necessárias para conexão do dispositivo com a cloud são fornecidas. Copie e guarde bem essas informações.
 
![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/11.png "Resumo de informações do dispositivo.")
Figura 11 - Resumo de informações do dispositivo.

Agora, copie a informação do “ID da organização” e substitua no valor do define ORG na aplicação embarcada. Depois faça o mesmo com a informação de “token de autenticação” no *define TOKEN*. Depois, com a informação “Tipo de Dispositivo” no *define DEVICE_TYPE*. E por último, a informação “ID do dispositivo” no *define DEVICE_ID*.

Depois carregue a aplicação na placa. Feito isso, e depois de conectada, veremos na dashboard uma indicação que o dispositivo está conectado e enviando dados.

![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/12.png "Status de conexão do dispositivo.")
Figura 12 - Status de conexão do dispositivo.
 
Voltemos a nossa aplicação no NodeRed
----------------------------------------

A aplicação presente ainda é um exemplo obtido quando criada. Iremos removê-la e criar uma nova. Para isso, clique em qualquer um dos blocos e pressione as teclas “Crtl + A” e depois “delete”.

Na imagem abaixo temos a nossa aplicação NodeRed já formatada para a utilização em forma privada. Para usá-la, basta importar o código para sua aplicação. Para isso, acesse menu > Import > Clipboard e insira o código.
 
![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/13.png "Importando aplicação para o NodeRed.")
Figura 13 - Importando aplicação para o NodeRed.

Depois de importada, clique em “deploy” e então a aplicação já estará funcional. A aplicação contém alguns comentários explicando seu funcionamento.

Caso queira mudar a aplicação para receber os dados de forma pública, modifique o nó “IBM IoT App In” clicando duas vezes sobre ele para abrir suas configurações. Modifique o campo “Authentication” para “Quickstart” e insira o “Device Type” e o “Device Id” configurados em seu dispositivo.
 
![placeholder](https://raw.githubusercontent.com/djunho/djunho.github.io/master/Imagens/2017-07-11-Conectando-Bluemix-ESP8266/14.png "Aplicação final de NodeRed.")
Figura 14 - Aplicação final de NodeRed.

Para aprender um pouco mais sobre nodered, acesse o [site oficial](https://nodered.org/). É interessante que com essa aplicação em nodeRed podemos realizar diversas análises sobre os dados recebidos e desencadear ações sobre os sistemas implementados de forma visual. Para os mais hardcore ainda se pode inserir códigos em node.js, unindo o útil ao agradável. Uma ferramenta simples e prática que acelera o desenvolvimento de uma aplicação.
 

Final
------

Com este artigo, construímos uma aplicação simples que simula um dado (um contador) e envia através de sua conexão com a internet o dado para a cloud da IBM. Tudo isso, de forma simples, sem grandes complicações e com segurança.

Use os passos aqui tratados adaptando-os para seu projeto.

