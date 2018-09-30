---
id: 219
title: Como instalar wireless Broadcom no Fedora
date: 2011-07-19T19:20:08+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=219
permalink: /2011/07/19/instalando-broacom-hp-compaq-cq40-fedora-15/
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
  - automação
  - driver
  - linux
  - rede
  - shell
tags:
  - broadcom
  - fedora
  - wireless
---
> &#8220;O que torna o Fedora diferente?
  
> Nós acreditamos no valor do software livre, e lutamos para proteger e promover soluções que qualquer um pode usar e redistribuir.&#8221;
  
> Fonte: <http://fedoraproject.org/pt_BR/about-fedora>

O trecho acima é muito dignificante e entusiasta, entretanto pode gerar alguns problemas, como a não inclusão de módulos(drivers) com licenças que não atendem aqueles requisitos de liberdade do Fedora.

Tenho um notebook HP Compaq CQ40 e ao instalar o Fedora, mesmo as últimas versões como Fedora 13, 14 e 15, a minha placa de rede wireless não funciona, isto porque o módulo dela não é free, portanto ele não pode estar nos repositórios oficiais do Fedora.

<!--more-->

Para solucionar podemos incluir os repositórios free e nonfree do RPM Fusion e instalar os módulos necessários para a rede wireless.

Use o comando abaixo para adicionar os repositórios do RPM Fusion:

<div class="wp_codebox">
  <table>
    <tr id="p21930">
      <td class="code" id="p219code30">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-Uvh</span> http:<span style="color: #000000; font-weight: bold;">//</span>download1.rpmfusion.org<span style="color: #000000; font-weight: bold;">/</span>free<span style="color: #000000; font-weight: bold;">/</span>fedora<span style="color: #000000; font-weight: bold;">/</span>rpmfusion-free-release-stable.noarch.rpm
rpm <span style="color: #660033;">-Uvh</span> http:<span style="color: #000000; font-weight: bold;">//</span>download1.rpmfusion.org<span style="color: #000000; font-weight: bold;">/</span>nonfree<span style="color: #000000; font-weight: bold;">/</span>fedora<span style="color: #000000; font-weight: bold;">/</span>rpmfusion-nonfree-release-stable.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Agora para certificar que não estamos com versões desatualizadas, faça um update geral <del datetime="2011-07-19T22:37:50+00:00">isto dá um certo medo</del>:

<div class="wp_codebox">
  <table>
    <tr id="p21931">
      <td class="code" id="p219code31">
        <pre class="bash" style="font-family:monospace;">yum update</pre>
      </td>
    </tr>
  </table>
</div>

Feito isto podemos instalar os pacotes para disponibilizar os drivers das placas de rede wireless free e nonfree no seu sistema:

<div class="wp_codebox">
  <table>
    <tr id="p21932">
      <td class="code" id="p219code32">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> wl-kmod-common kmod-wl</pre>
      </td>
    </tr>
  </table>
</div>

Se tudo correu bem, seu hardware já deve estar disponível no sistema. Caso você não saiba como ativá-lo manualmente logo após instalar, experimente reiniciar o computador.