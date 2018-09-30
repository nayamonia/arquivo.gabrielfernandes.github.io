---
id: 1567
title: Google Earth não abre no Fedora x86_64
date: 2012-07-31T12:40:31+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1567
permalink: /2012/07/31/google-earth-nao-abre-fedora-x86_64/
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
image: /wp-content/uploads/2012/07/google-earth.jpg
categories:
  - linux
  - vídeos
tags:
  - fedora
  - google earth
  - x86_64
---
Estava eu instalando as coisas no meu Fedora 17 x86_64 recém instalado, quando cheguei na instalação do Google Earth&#8230;

> Instalar o Google Earth é muito simples, acesse <a href="http://www.google.com.br/earth" title="Eu vou te levar para a página do Google Earth, não esqueça de voltar!" target="_blank">http://www.google.com.br/earth</a> (para pt-BR) clique no botão &#8220;Download do Google Earth&#8221; e depois selecione a versão que deseja baixar. Eu baixei a **.rpm de 64 bits (Para Fedora/openSUSE)** e instalei assim:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p1567256">
>       <td class="code" id="p1567code256">
>         <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> google-earth-stable_current_x86_64.rpm</pre>
>       </td>
>     </tr>
>   </table>
> </div>

<!--more [CONTINUAR LENDO]-->

Na instalação, aparentemente, tudo correu bem, mas ao tentar abrir o Google Earth o mesmo não abria, experimentei abrir pelo terminal para ver o que acontecia e descobri que havia um erro como este que segue:

<div class="wp_codebox">
  <table>
    <tr id="p1567257">
      <td class="code" id="p1567code257">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>bin<span style="color: #000000; font-weight: bold;">/</span>google-earth: .<span style="color: #000000; font-weight: bold;">/</span>googleearth-bin: <span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>ld-lsb.so.3: bad ELF interpreter: Arquivo ou diretório não encontrado</pre>
      </td>
    </tr>
  </table>
</div>

Opa! Parece estar faltando alguma dependência, vamos perguntar para o _yum_ a qual pacote pertence a biblioteca _/lib/ld-lsb.so.3_ assim:

<div class="wp_codebox">
  <table>
    <tr id="p1567258">
      <td class="code" id="p1567code258">
        <pre class="bash" style="font-family:monospace;">yum provides <span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>ld-lsb.so.3</pre>
      </td>
    </tr>
  </table>
</div>

Um resultado como este deve estar sendo exibido no seu terminal:

<div class="wp_codebox">
  <table>
    <tr id="p1567259">
      <td class="code" id="p1567code259">
        <pre class="bash" style="font-family:monospace;">Plugins carregados: fastestmirror, langpacks, presto, refresh-packagekit
google-earth                                                       <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000;">951</span> B     00:00 <span style="color: #000000; font-weight: bold;">!!!</span> 
Loading mirror speeds from cached hostfile
 <span style="color: #000000; font-weight: bold;">*</span> fedora: ftp.linux.ncsu.edu
 <span style="color: #000000; font-weight: bold;">*</span> rpmfusion-free: mirror.hiwaay.net
 <span style="color: #000000; font-weight: bold;">*</span> rpmfusion-free-updates: mirror.hiwaay.net
 <span style="color: #000000; font-weight: bold;">*</span> rpmfusion-nonfree: mirror.hiwaay.net
 <span style="color: #000000; font-weight: bold;">*</span> rpmfusion-nonfree-updates: mirror.hiwaay.net
 <span style="color: #000000; font-weight: bold;">*</span> updates: ftp.linux.ncsu.edu
google-earth<span style="color: #000000; font-weight: bold;">/</span>filelists                                             <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">3.7</span> kB     00:00 <span style="color: #000000; font-weight: bold;">!!!</span> 
rpmfusion-free-updates<span style="color: #000000; font-weight: bold;">/</span>filelists_db                                <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">105</span> kB     00:01     
updates<span style="color: #000000; font-weight: bold;">/</span>filelists_db                                               <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">8.6</span> MB     01:09     
redhat-lsb-<span style="color: #000000;">4.0</span>-<span style="color: #000000;">11</span>.fc17.i686 : LSB base libraries support <span style="color: #000000; font-weight: bold;">for</span> Red Hat Enterprise Linux
Repo        : fedora
Resultado a partir de:
Nome de arquivo    : <span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>ld-lsb.so.3
&nbsp;
redhat-lsb-<span style="color: #000000;">4.1</span>-<span style="color: #000000;">5</span>.fc17.i686 : Implementation of Linux Standard Base specification
Repo        : updates
Resultado a partir de:
Nome de arquivo    : <span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>ld-lsb.so.3</pre>
      </td>
    </tr>
  </table>
</div>

Feito, nosso amigo cachorro amarelo mostrou que a biblioteca _/lib/ld-lsb.so.3_ pertence ao pacote _redhat-lsb_. O detalhe é que meu Fedora é x86_64 e o pacote exigido pelo **Google Earth** 64bits exige uma biblioteca existente em um pacote i686 (32bits), fazer o que né, vamos instalar a _redhat-lsb_ i686, pois é provável que tu já tenhas a _redhat-lsb_ **x86_64**, mas ela não resolve nosso problema.

Para acabar com esta história e ser feliz com o **Google Earth** 64bits, execute:

<div class="wp_codebox">
  <table>
    <tr id="p1567260">
      <td class="code" id="p1567code260">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> redhat-lsb.i686</pre>
      </td>
    </tr>
  </table>
</div>

Segue vídeo com todos os passos executados neste post:

<div class="jetpack-video-wrapper">
  <span class="embed-youtube" style="text-align:center; display: block;"></span>
</div>

Boa sorte!