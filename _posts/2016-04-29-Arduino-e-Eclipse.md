---
layout: post
title: "Onde o Arduino se encontra com o Eclipse IDE"
tags: 
  - Arduino
  - Genuino
  - IDE
  - Eclipse
  - Plugin
---

Como um bom engenheiro, gosto muito de "fuçar" nas coisas para entender como elas funcionam. Pelo menos eu tento e muitas vezes eu falho. Perdi a conta de quantos brinquedos que abri e nunca mais funcionaram da mesma forma. Tento entender as coisas de sistemas embarcados, placas, componentes, software, pessoas, comportamento, empresas, etc. Com o arduino isso nao é diferente.

<!-- more -->

Desde que me formei, trabalho com sistemas embarcados no desenvolvimento de firmwares diversos. Trabalho em C, e raramente preciso me aventurar no _assembly_ para essa função. Como trabalho com IDE's mais completas sinto falta disso no arduino. Então, porque não juntar o útil ao agradável!?

Sei que o Atmel Studio, IDE provida pela atmel (agora Microchip) tem suporte para o Arduino. Mas como em casa utilizo linux, preferi procurar algo neste cenário. Também para verificar como andam outras soluções _open sources_.

Antes de sair me aventurando pela web ou pelos códigos da vida, resolvi dar uma olhada nas coisas que já existiam. E eis que encontro uma solução para o Eclipse, muito bem elaborada por sinal. [Veja aqui](http://eclipse.baeyens.it).

Vale a pena dar uma analisada. Afinal, é uma interface já "amigável" e já conhecida de muitos, e vem toda configurada.

![placeholder](/assets/images/2016-04-29-EclipseArduino/printscreen_ArduinoEclipse.png "Print screen do Eclipse com plugin do Arduino, claro que rodando um Blink")

No site do plugin tem uma documentação boa e alguns videos (no mínimo engraçados envolvendo gatos) sobre o plugin. Com toda essa base, é facil botar para rodar os programas de exemplo. Lógicamente rodei um blink, quem não faria o mesmo?

Com essa nova ferramenta, podemos ver e analisar de forma simples e com o apoio de uma IDE a complexidade que existe por trás do arduino. Podemos dar um _"Open Declaration"_ e ver o conteúdo das funções disponíveis pelo Arduino. Como por exemplo do `digitalWrite()`.

{% highlight C %}
void digitalWrite(uint8_t pin, uint8_t val)
{
	uint8_t timer = digitalPinToTimer(pin);
	uint8_t bit = digitalPinToBitMask(pin);
	uint8_t port = digitalPinToPort(pin);
	volatile uint8_t *out;

	if (port == NOT_A_PIN) return;

	// If the pin that support PWM output, we need to turn it off
	// before doing a digital write.
	if (timer != NOT_ON_TIMER) turnOffPWM(timer);

	out = portOutputRegister(port);

	uint8_t oldSREG = SREG;
	cli();

	if (val == LOW) {
		*out &= ~bit;
	} else {
		*out |= bit;
	}

	SREG = oldSREG;
}
{% endhighlight %}

Um fato interessante é que, em um dos vídeos o autor mostra que foi possível simplificar o plugin (a partir da versão 3) pelo fato da Arduino criar o conceito da _Boards_ e da _Library Manager_ (que surgiu na versão 1.6 do Arduino IDE), o que facilitou o acesso aos arquivos de terceiros. Desta forma o autor implementou seu proprio gerenciador permitindo incluir placas e bibliotecas de terceiros com essa funcionalidade no Eclipse. Muito legal.
