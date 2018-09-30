---
id: 511
title: Como alterar configuração de rede automática da wireless no Gnome 3 Fedora 15
date: 2011-08-22T18:34:56+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=511
permalink: /2011/08/22/como-alterar-configuracao-de-rede-automatica-da-wireless-no-gnome-3-fedora-15/
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
  - imagens
  - linux
  - rede
tags:
  - fedora
  - gnome
  - linux
  - rede
  - wireless
---
A interface padrão para configuração de redes no **Gnome 3** possui um bug que não permite alterar as configurações das redes **wireless** automáticas, pois o botão _Opções_ fica desabilitado. 

Como alternativa podemos usar o configurador legado, o mesmo utilizado na versão anterior do **Gnome**.

Execute na console:

<!--more [CONTINUAR LENDO]-->

<div class="wp_codebox">
  <table>
    <tr id="p511104">
      <td class="code" id="p511code104">
        <pre class="bash" style="font-family:monospace;">nm-connection-editor</pre>
      </td>
    </tr>
  </table>
</div>

Uma tela como esta a seguir deve ser exibida. Este programa possui uma interface bastante intuitiva. Em resumo você deve selecionar a rede e _Editar_ as configurações da wireless:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6081/6070572115_29fe5012b5_z.jpg?resize=566%2C341&#038;ssl=1" alt="nm-connection-editor no Gnome 3" width="566" height="341" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6070572115/)

Caso tenha dificuldades, favor comentar.