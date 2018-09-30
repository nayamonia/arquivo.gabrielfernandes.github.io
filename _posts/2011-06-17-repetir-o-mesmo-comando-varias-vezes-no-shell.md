---
id: 3
title: Repetir o mesmo comando várias vezes no shell
date: 2011-06-17T16:45:23+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=3
permalink: /2011/06/17/repetir-o-mesmo-comando-varias-vezes-no-shell/
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
ratings_users:
  - "0"
ratings_score:
  - "0"
ratings_average:
  - "0"
categories:
  - linux
  - shell
tags:
  - shell
---
Por vezes queremos acompanhar a cópia de um arquivo na console do Linux e o caminho mais normal é abrir um outro terminal e ficar repetitivamente executando o comando **ls**, ou algum outro comando, haja dedo para apertar a seta pra cima e enter, seta pra cima e enter, seta pra cima e enter, coisa bem chatinha esta, sem contar o fato de encher o histórico de comandos com linhas repetidas, dificultando encontrar um comando feito anteriormente.

Podemos resolver isto usando o comando **while** de forma bem simples, por exemplo se quisermos executar um **ls** por várias vezes, podemos fazer assim:

<!--more-->

<div class="wp_codebox">
  <table>
    <tr id="p36">
      <td class="code" id="p3code6">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">true</span>; <span style="color: #000000; font-weight: bold;">do</span> <span style="color: #c20cb9; font-weight: bold;">ls</span>; <span style="color: #000000; font-weight: bold;">done</span>;</pre>
      </td>
    </tr>
  </table>
</div>

Isto vai executar o comando **ls** até pressionarmos CTRL + C para quebrá-lo, entretanto o comando desta forma, não terá muita utilidade, pois vai mostrar o resultado do **ls** e em seguida fazer outro **ls** e assim sucessivamente, logo o resultado será ilegível para humanos normais, digamos assim.

Para resolver podemos dar uma pequena pausa entre cada iteração do **while** valendo-se do comando **sleep**. No exemplo que segue, vamos executar o comando **ls**, aguardar 10 segundos (**sleep 10**) e executar novamente o **ls** e depois a pausa novamente e assim por diante.

<div class="wp_codebox">
  <table>
    <tr id="p37">
      <td class="code" id="p3code7">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">true</span>; <span style="color: #000000; font-weight: bold;">do</span> <span style="color: #c20cb9; font-weight: bold;">ls</span>; <span style="color: #c20cb9; font-weight: bold;">sleep</span> <span style="color: #000000;">10</span>; <span style="color: #000000; font-weight: bold;">done</span>;</pre>
      </td>
    </tr>
  </table>
</div>

Agora já temos um resultado mais interessante e podemos apenas observar o que esta acontecendo, economizando bastante tecladas !!!

Por fim, para deixar o coisa mais bonita, podemos usar o comando **clear** para limpar a tela antes de executar novamente o **ls**, ai nosso resultado será bem legível para humanos normais. 😉

Veja como ficou nosso comando agora:

<div class="wp_codebox">
  <table>
    <tr id="p38">
      <td class="code" id="p3code8">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">true</span>; <span style="color: #000000; font-weight: bold;">do</span> <span style="color: #c20cb9; font-weight: bold;">ls</span>; <span style="color: #c20cb9; font-weight: bold;">sleep</span> <span style="color: #000000;">10</span>; <span style="color: #c20cb9; font-weight: bold;">clear</span>; <span style="color: #000000; font-weight: bold;">done</span>;</pre>
      </td>
    </tr>
  </table>
</div>