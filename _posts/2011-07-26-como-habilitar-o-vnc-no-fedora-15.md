---
id: 272
title: Como configurar o VNC no Fedora 15
date: 2011-07-26T08:06:05+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=272
permalink: /2011/07/26/como-habilitar-o-vnc-no-fedora-15/
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
categories:
  - linux
  - rede
  - shell
tags:
  - console
  - fedora
  - linux
  - shell
  - terminal
  - vnc
  - x11vnc
  - yum
---
Isto é algo verdadeiramente útil e simples de ativar, acompanhe os passos:

Instale o pacote **x11vnc**, use o usuário **root**:

<div class="wp_codebox">
  <table>
    <tr id="p27242">
      <td class="code" id="p272code42">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> x11vnc</pre>
      </td>
    </tr>
  </table>
</div>

Com o usuário comum, o mesmo que está logado no ambiente gráfico, execute o comando abaixo e cadastre uma senha:

<div class="wp_codebox">
  <table>
    <tr id="p27243">
      <td class="code" id="p272code43">
        <pre class="bash" style="font-family:monospace;">vncpasswd</pre>
      </td>
    </tr>
  </table>
</div>

Ative o VNC na tela padrão(**display 0**): 

<!--more-->

<div class="wp_codebox">
  <table>
    <tr id="p27244">
      <td class="code" id="p272code44">
        <pre class="bash" style="font-family:monospace;">x11vnc <span style="color: #660033;">-rfbauth</span> ~<span style="color: #000000; font-weight: bold;">/</span>.vnc<span style="color: #000000; font-weight: bold;">/</span><span style="color: #c20cb9; font-weight: bold;">passwd</span> <span style="color: #660033;">-display</span> :<span style="color: #000000;"></span> <span style="color: #000000; font-weight: bold;">&</span></pre>
      </td>
    </tr>
  </table>
</div>

Não podemos esquecer que para se conectar é necessário desligar o firewall ou configurar para aceitar conexões pela porta padrão do **VNC 5900**. 

Para os mais experientes basta incluir as duas linhas abaixo no seu arquivo **iptables**, no caso do Fedora é o arquivo **/etc/sysconfig/iptables** e reiniciar o **iptables**:

<div class="wp_codebox">
  <table>
    <tr id="p27245">
      <td class="code" id="p272code45">
        <pre class="bash" style="font-family:monospace;"><span style="color: #660033;">-A</span> INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">5900</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">5900</span> <span style="color: #660033;">-j</span> ACCEPT</pre>
      </td>
    </tr>
  </table>
</div>

Se você não se sente seguro para alterar o arquivo na unha, pode fazer pelo programa **setup**, assim:

<div class="wp_codebox">
  <table>
    <tr id="p27246">
      <td class="code" id="p272code46">
        <pre class="bash" style="font-family:monospace;">setup</pre>
      </td>
    </tr>
  </table>
</div>

Selecione **Configuração do Firewall**, **Personalizar**, **Avançar** e então **Adicionar**.
  
Adicione a porta **5900** para **tcp** e **udp**, após adicionado deve mostrar uma tela como esta a seguir:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6139/5978056113_e9a70b039b_z.jpg?resize=640%2C423&#038;ssl=1" alt="Portas adicionadas 5900:tcp e 5900:udp" width="640" height="423" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5978056113/)

Agora selecione **Fechar** e depois **Ok**. Se tudo correu bem devemos ter apresentada agora uma tela como esta que segue:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6137/5978056121_e8f8bf440c_z.jpg?resize=640%2C422&#038;ssl=1" alt="Finalizando e salvando novas regras do iptables" width="640" height="422" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5978056121/)

Nesta acima, escolha **Sim** e para finalizar reinicie o **iptables** com o comando abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p27247">
      <td class="code" id="p272code47">
        <pre class="bash" style="font-family:monospace;">service iptables restart</pre>
      </td>
    </tr>
  </table>
</div>

Se chegou até aqui, você já deve ter o VNC funcionando e aceitando conexões remotas.