---
id: 148
title: Instalar Oracle VirtualBox pelo YUM no Fedora
date: 2011-07-05T18:44:09+00:00
author: Gabriel Fernandes
layout: post
guid: http://cd2.com.br/?p=148
permalink: /2011/07/05/instalar-oracle-virtualbox-pelo-yum-no-fedora/
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
  - virtualização
tags:
  - fedora
  - oracle
  - virtualbox
  - yum
---
Mais uma vez digo: A melhor maneira de instalar e manter atualizado os programas no Fedora, sem dúvida é usando o YUM.

Portanto esta dica é para àqueles que desejam conforto na instalação e atualização do Oracle VirtualBox, digo conforto porque baixar o pacote RPM disponível no site não garante que teremos um ambiente que atenda todos as dependências necessárias para a instalação, mas como o YUM isto se resolve automaticamente, pois ele vai identificar as dependências e instalar tudo de uma só vez. 

Assim para termos mais esta facilidade no YUM, devemos adicionar o repositório do Oracle VirtualBox, para tanto façamos o seguinte:

<!--more-->

1- Criaremos um arquivo chamando **virtualbox.repo** na pasta **/etc/yum.repos.d**;

2- Agora vamos adicionar as linhas abaixo no arquivo e salvar, sugiro o uso do **vi** para tal:

<div class="wp_codebox">
  <table>
    <tr id="p14823">
      <td class="code" id="p148code23">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>virtualbox<span style="color: #7a0874; font-weight: bold;">&#93;</span>
<span style="color: #007800;">name</span>=Fedora <span style="color: #007800;">$releasever</span> - <span style="color: #007800;">$basearch</span> - VirtualBox
<span style="color: #007800;">baseurl</span>=http:<span style="color: #000000; font-weight: bold;">//</span>download.virtualbox.org<span style="color: #000000; font-weight: bold;">/</span>virtualbox<span style="color: #000000; font-weight: bold;">/</span>rpm<span style="color: #000000; font-weight: bold;">/</span>fedora<span style="color: #000000; font-weight: bold;">/</span><span style="color: #007800;">$releasever</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #007800;">$basearch</span>
<span style="color: #007800;">enabled</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgcheck</span>=<span style="color: #000000;">1</span>
<span style="color: #007800;">gpgkey</span>=http:<span style="color: #000000; font-weight: bold;">//</span>download.virtualbox.org<span style="color: #000000; font-weight: bold;">/</span>virtualbox<span style="color: #000000; font-weight: bold;">/</span>debian<span style="color: #000000; font-weight: bold;">/</span>oracle_vbox.asc</pre>
      </td>
    </tr>
  </table>
</div>

3- Para instalar agora, podemos usar o comando:

<div class="wp_codebox">
  <table>
    <tr id="p14824">
      <td class="code" id="p148code24">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> VirtualBox-<span style="color: #000000;">4.1</span></pre>
      </td>
    </tr>
  </table>
</div>

* Tudo deve ser feito com o usuário root

** Se você executou a instalação com sucesso, mas não consegue executar as máquinas virtuais e obtem um erro ao executar _/etc/init.d/vboxdrv setup_ leia o post [**Erro ao executar ‘/etc/init.d/vboxdrv setup’ no Oracle VirtualBox**](http://cd2.com.br/2011/07/18/erro-ao-executar-etcinit-dvboxdrv-setup-no-oracle-virtualbox/).

\*** Para Fedora 19 use a **versão 4.2**, invés da 4.1:

<div class="wp_codebox">
  <table>
    <tr id="p14825">
      <td class="code" id="p148code25">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> VirtualBox-<span style="color: #000000;">4.2</span></pre>
      </td>
    </tr>
  </table>
</div>