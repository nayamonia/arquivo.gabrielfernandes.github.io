---
id: 5
title: Converter disco virtual VMDK para VDI em 3 passos
date: 2011-06-15T13:29:14+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=5
permalink: /2011/06/15/converter-disco-virtual-vmdk-para-vdi-em-3-passos/
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
  - virtualização
tags:
  - disco virtual
  - vdi
  - vmdk
---
Quer migrar seus discos rígidos de uma máquina virtual da VMware para o VirtualBox?

Veja como pode ser simples:

1. Primeiro usaremos o qemu-img para converter no formato RAW:

<div class="wp_codebox">
  <table>
    <tr id="p51">
      <td class="code" id="p5code1">
        <pre class="bash" style="font-family:monospace;">qemu-img convert arquivo.vmdk <span style="color: #660033;">-O</span> raw arquivo.bin</pre>
      </td>
    </tr>
  </table>
</div>

2. Agora vamos converter de RAW para VDI:

<!--more-->

<div class="wp_codebox">
  <table>
    <tr id="p52">
      <td class="code" id="p5code2">
        <pre class="bash" style="font-family:monospace;">VBoxManage convertdd xxxxx.bin xxxxxx.vdi</pre>
      </td>
    </tr>
  </table>
</div>

3. Por fim, vamos compactar o VDI:

<div class="wp_codebox">
  <table>
    <tr id="p53">
      <td class="code" id="p5code3">
        <pre class="bash" style="font-family:monospace;">VBoxManage modifyvdi xxxxxx.vdi compact.</pre>
      </td>
    </tr>
  </table>
</div>