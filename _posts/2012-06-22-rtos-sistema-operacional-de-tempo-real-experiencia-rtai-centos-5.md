---
id: 1520
title: 'RTOS &#8211; Sistema operacional de tempo real, experiência com RTAI no CentOS 5'
date: 2012-06-22T13:09:56+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1520
permalink: /2012/06/22/rtos-sistema-operacional-de-tempo-real-experiencia-rtai-centos-5/
pv_video:
  - ""
pv_height:
  - "200"
pv_width:
  - "300"
pv_float:
  - right
pv_in_post:
  - "0"
pv_in_widget:
  - "0"
pv_video_text:
  - ""
image: /wp-content/uploads/2012/06/Website-002-Hunting-009.jpg
categories:
  - desenvolvimento
  - driver
  - linux
  - shell
  - virtualização
tags:
  - centos
  - rtos
  - sistema operacional
---
Tive que fazer uma experiência com sistemas operacionais de tempo real para um trabalho da faculdade e tomei uma surra para encontrar um sistema tempo real para download prontinho para testar &#8230; então resolvi montar meu próprio ambiente RTAI em cima de uma instalação de CentOS.

Gravei toda minha experiencia em vídeos e postei no youtube, não tem áudio, nem edição, pois não deu tempo pra fazer, segue:

1- No primeiro vídeo faço a instalação mínima do CentOS, bem rapidinho 10 minutos&#8230;

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

<!--more [CONTINUAR LENDO]-->

2- Agora a parte mais demorada que foi aplicar o patch RTAI no fonte, configurar , recompilar e instalar um novo Kernel com suporte ao RTAI. Este vídeo tem uma hora e dez minutos, o tempo de compilação é bastante grande, se for assistir sugiro pular a parte da compilação, segue:

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

3- Por fim, configurei, compilei e instalei o RTAI e fiz um teste bobão, segue:

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

Conclusão, para instalar um ambiente real precisa pesquisar mais &#8230; mas para efeito didático é o suficiente para mim.