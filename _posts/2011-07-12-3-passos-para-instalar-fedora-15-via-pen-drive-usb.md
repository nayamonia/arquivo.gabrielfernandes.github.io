---
id: 170
title: 3 passos para instalar Fedora 15 via pen drive USB
date: 2011-07-12T18:22:07+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=170
permalink: /2011/07/12/3-passos-para-instalar-fedora-15-via-pen-drive-usb/
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
  - shell
tags:
  - fedora
  - linux
  - pen drive
  - usb
---
Faça download da versão live do Fedora 15 de sua preferência no site oficial do Fedora. O link abaixo leva direto para a área de download:

[http://fedoraproject.org/pt_BR/get-fedora](http://fedoraproject.org/pt_BR/get-fedora "Download Fedora")

Eu baixei a versão sugerida por eles, àquela quem vem com Gnome, alias o Gnome 3 tá muito legal, voltando ao download, baixei o de 565 MB, imagem no formato ISO para PCs compatíveis com Intel (32-bit).

Após baixar você deve ter um arquivo ISO no seu disco com nome parecido com este **Fedora-15-i686-Live-Desktop.iso**, como ele podemos queimar um CD, instalar e jogar o CD fora. Mas quem não quer desperdiçar um CD, pode instalar pelo pen drive USB, veja como:

<!--more-->

1- Conecte um pen drive vazio ou que possa ser formatado em um sistema Linux, sugiro o próprio Fedora e identifique em qual dispositivo o pen drive USB foi disponibilizado, no meu caso foi em /dev/sdc. Você pode usar o comando **mount** para ver isto;

2- Sabendo o endereço do dispositivo no sistema, com o programa **livecd-iso-to-disk** podemos criar nosso live pen drive, executando o comando que segue com o usuário **root**. IMPORTANTE: este é um exemplo, estamos FORMATANDO tudo que estiver no pen drive localizado no endereço **/dev/sdc**, tenha atenção, pois poderá ser diferente no seu computador;

<div class="wp_codebox">
  <table>
    <tr id="p17026">
      <td class="code" id="p170code26">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>bin<span style="color: #000000; font-weight: bold;">/</span>livecd-iso-to-disk <span style="color: #660033;">--format</span> <span style="color: #660033;">--reset-mbr</span> Fedora-<span style="color: #000000;">15</span>-i686-Live-Desktop.iso <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>sdc</pre>
      </td>
    </tr>
  </table>
</div>

3- Em alguns instantes será solicitado a confirmação da formatação do dispositivo, pressione ENTER para confirmar e CTRL+C para abortar sem perder qualquer informação do pen drive. Se respondeu SIM (pressionou ENTER), aguarde mais alguns minutos que seu pen drive estará pronto para rodar o Fedora 15 Live e a partir dele, você pode instalá-lo normalmente como faria usando um CD.