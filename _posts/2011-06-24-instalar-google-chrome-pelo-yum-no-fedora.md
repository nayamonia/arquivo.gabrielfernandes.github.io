---
id: 88
title: Instalar Google Chrome pelo YUM no Fedora
date: 2011-06-24T13:07:36+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=88
permalink: /2011/06/24/instalar-google-chrome-pelo-yum-no-fedora/
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
  - linux
  - shell
tags:
  - fedora
  - google chrome
  - yum
---
A melhor maneira de instalar e manter atualizado os programas no Fedora, sem dúvida é usando o YUM.

O [download padrão da Google](http://www.google.com/chrome?hl=pt-BR) oferece um pacote RPM para a instalação e nem sempre temos um ambiente que atenda todos as dependências necessárias para a instalação.

Mais um motivo para usarmos o YUM, pois ele mesmo resolve as dependências tornando a tarefa muito mais confortável e segura.

Portanto para termos esta facilidade no YUM, devemos adicionar o repositório do Google Chrome, para tanto façamos o seguinte:

<!--more-->

1- Criaremos um arquivo chamando **google-chrome.repo** na pasta **/etc/yum.repos.d** com o comando:

<div class="wp_codebox">
  <table>
    <tr id="p889">
      <td class="code" id="p88code9">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">touch</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>yum.repos.d<span style="color: #000000; font-weight: bold;">/</span>google-chrome.repo</pre>
      </td>
    </tr>
  </table>
</div>

2- Agora vamos adicionar as linhas abaixo no arquivo e salvar. 

Para 32-bits:

<div class="wp_codebox">
  <table>
    <tr id="p8810">
      <td class="code" id="p88code10">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>google<span style="color: #7a0874; font-weight: bold;">&#93;</span>
<span style="color: #007800;">name</span>=Google
<span style="color: #007800;">baseurl</span>=http:<span style="color: #000000; font-weight: bold;">//</span>dl.google.com<span style="color: #000000; font-weight: bold;">/</span>linux<span style="color: #000000; font-weight: bold;">/</span>rpm<span style="color: #000000; font-weight: bold;">/</span>stable<span style="color: #000000; font-weight: bold;">/</span>i386
<span style="color: #007800;">enabled</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgcheck</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgkey</span>=https:<span style="color: #000000; font-weight: bold;">//</span>dl-ssl.google.com<span style="color: #000000; font-weight: bold;">/</span>linux<span style="color: #000000; font-weight: bold;">/</span>linux_signing_key.pub</pre>
      </td>
    </tr>
  </table>
</div>

Para 64-bits:

<div class="wp_codebox">
  <table>
    <tr id="p8811">
      <td class="code" id="p88code11">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>google64<span style="color: #7a0874; font-weight: bold;">&#93;</span>
<span style="color: #007800;">name</span>=Google
<span style="color: #007800;">baseurl</span>=http:<span style="color: #000000; font-weight: bold;">//</span>dl.google.com<span style="color: #000000; font-weight: bold;">/</span>linux<span style="color: #000000; font-weight: bold;">/</span>rpm<span style="color: #000000; font-weight: bold;">/</span>stable<span style="color: #000000; font-weight: bold;">/</span>x86_64
<span style="color: #007800;">enabled</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgcheck</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgkey</span>=https:<span style="color: #000000; font-weight: bold;">//</span>dl-ssl.google.com<span style="color: #000000; font-weight: bold;">/</span>linux<span style="color: #000000; font-weight: bold;">/</span>linux_signing_key.pub</pre>
      </td>
    </tr>
  </table>
</div>

3- Podemos usar o **vi** para tal:

<div class="wp_codebox">
  <table>
    <tr id="p8812">
      <td class="code" id="p88code12">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>yum.repos.d<span style="color: #000000; font-weight: bold;">/</span>google-chrome.repo</pre>
      </td>
    </tr>
  </table>
</div>

4- Para instalar agora podemos usar o comando:

<div class="wp_codebox">
  <table>
    <tr id="p8813">
      <td class="code" id="p88code13">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> google-chrome-stable</pre>
      </td>
    </tr>
  </table>
</div>

* Tudo deve ser feito com o usuário root