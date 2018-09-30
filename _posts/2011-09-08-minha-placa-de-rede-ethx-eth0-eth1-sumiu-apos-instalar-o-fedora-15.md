---
id: 770
title: 'Minha placa de rede ethX (eth0, eth1, &#8230;) sumiu após instalar o Fedora 15'
date: 2011-09-08T14:18:31+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=770
permalink: /2011/09/08/minha-placa-de-rede-ethx-eth0-eth1-sumiu-apos-instalar-o-fedora-15/
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
  - eth0
  - fedora
  - placa de rede
---
Você acabou de **instalar o Fedora 15** e não encontra mais sua **placa de rede ethX (eth0, eth1, &#8230;)** no comando _ifconfig_?

Não há motivos para pânico, sua placa de rede deve estar sendo exibida no comando _ifconfig_, porém com outro nome. Isto se deve ao fato de que a partir do **Fedora 15**, foi implementado um novo recurso chamado _Consistent Network Device Naming_ (_Nomeação Consistente de Dispositivos de Rede_) para nomear as placas de rede.

<!--more [CONTINUAR LENDO]-->

O novo método altera o esquema de nomeação de dispositivo de rede para um baseado no hardware facilitando a identificação e utilização. O motivo para esta modificação está relacionado ao fato de que a forma atual de nomear estes dispositivos, quando há mais de uma placa de rede, pode gerar problemas alterando os nomes das placas numa reinstalação e em alguns casos após uma reinicialização do sistema. Em termos práticos quero dizer: se você tem duas **placas de rede**, aquela placa atribuída como **eth0** pode receber o nome de **eth1** e esta consequentemente tornar-se a **eth0** após uma reinstalação ou reinício do sistema gerando problemas na configuração. 

O novo sistema deve nomear as **placas de rede**, da seguinte forma:

  * Para as **placas de rede** on-board:
<div class="wp_codebox">
  <table>
    <tr id="p770118">
      <td class="code" id="p770code118">
        <pre class="bash" style="font-family:monospace;">em<span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">1234</span><span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

  * Para as **placas de rede** PCI:
<div class="wp_codebox">
  <table>
    <tr id="p770119">
      <td class="code" id="p770code119">
        <pre class="bash" style="font-family:monospace;">p<span style="color: #000000; font-weight: bold;">&lt;</span>slot<span style="color: #000000; font-weight: bold;">&gt;</span>p<span style="color: #000000; font-weight: bold;">&lt;</span>porta<span style="color: #000000; font-weight: bold;">&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Você pode desabilitar este recurso desligando o parâmetro _biosdevname_ do _Kernel_.

Com o parâmetro ligado, veja como estava a saída do comando _ifconfig_ na minha máquina:

<div class="wp_codebox">
  <table>
    <tr id="p770120">
      <td class="code" id="p770code120">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>sbin<span style="color: #000000; font-weight: bold;">/</span><span style="color: #c20cb9; font-weight: bold;">ifconfig</span></pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p770121">
      <td class="code" id="p770code121">
        <pre class="bash" style="font-family:monospace;">lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::<span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">128</span> Scope:Host
          UP LOOPBACK RUNNING  MTU:<span style="color: #000000;">16436</span>  Metric:<span style="color: #000000;">1</span>
          RX packets:<span style="color: #000000;"></span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> frame:<span style="color: #000000;"></span>
          TX packets:<span style="color: #000000;"></span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> carrier:<span style="color: #000000;"></span>
          collisions:<span style="color: #000000;"></span> txqueuelen:<span style="color: #000000;"></span> 
          RX bytes:<span style="color: #000000;"></span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">0.0</span> b<span style="color: #7a0874; font-weight: bold;">&#41;</span>  TX bytes:<span style="color: #000000;"></span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">0.0</span> b<span style="color: #7a0874; font-weight: bold;">&#41;</span>
&nbsp;
p2p1      Link encap:Ethernet  HWaddr 08:00:<span style="color: #000000;">27</span>:<span style="color: #000000;">34</span>:8E:3F  
          inet addr:192.168.254.65  Bcast:192.168.254.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe34:8e3f<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">64</span> Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:<span style="color: #000000;">1500</span>  Metric:<span style="color: #000000;">1</span>
          RX packets:<span style="color: #000000;">1187</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> frame:<span style="color: #000000;"></span>
          TX packets:<span style="color: #000000;">38</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> carrier:<span style="color: #000000;"></span>
          collisions:<span style="color: #000000;"></span> txqueuelen:<span style="color: #000000;">1000</span> 
          RX bytes:<span style="color: #000000;">99554</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">97.2</span> KiB<span style="color: #7a0874; font-weight: bold;">&#41;</span>  TX bytes:<span style="color: #000000;">10787</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">10.5</span> KiB<span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

Observe que minha **placa de rede** está com o nome _p2p1_.
  
Para desabilitar este recurso edite o arquivo _/boot/grub/grub.conf_ e adicione na linha de comando do _Kernel_ o parâmetro _biosdevname=0_. A seguir segue exemplo de como ficaria o arquivo com este parâmetro:

<div class="wp_codebox">
  <table>
    <tr id="p770122">
      <td class="code" id="p770code122">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Note that you do not have to rerun grub after making changes to this file</span>
<span style="color: #666666; font-style: italic;"># NOTICE:  You have a /boot partition.  This means that</span>
<span style="color: #666666; font-style: italic;">#          all kernel and initrd paths are relative to /boot/, eg.</span>
<span style="color: #666666; font-style: italic;">#          root (hd0,0)</span>
<span style="color: #666666; font-style: italic;">#          kernel /vmlinuz-version ro root=/dev/mapper/vg_builder-lv_root</span>
<span style="color: #666666; font-style: italic;">#          initrd /initrd-[generic-]version.img</span>
<span style="color: #666666; font-style: italic;">#boot=/dev/sda</span>
<span style="color: #007800;">default</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">timeout</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">splashimage</span>=<span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>splash.xpm.gz
hiddenmenu
title Fedora <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.40.3-<span style="color: #000000;"></span>.fc15.i686<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.40.3-<span style="color: #000000;"></span>.fc15.i686 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>mapper<span style="color: #000000; font-weight: bold;">/</span>vg_builder-lv_root <span style="color: #007800;">rd_LVM_LV</span>=vg_builder<span style="color: #000000; font-weight: bold;">/</span>lv_root <span style="color: #007800;">rd_LVM_LV</span>=vg_builder<span style="color: #000000; font-weight: bold;">/</span>lv_swap rd_NO_LUKS rd_NO_MD rd_NO_DM <span style="color: #007800;">LANG</span>=en_US.UTF-<span style="color: #000000;">8</span> <span style="color: #007800;">SYSFONT</span>=latarcyrheb-sun16 <span style="color: #007800;">KEYTABLE</span>=pt-latin1 <span style="color: #007800;">biosdevname</span>=<span style="color: #000000;"></span> rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initramfs-2.6.40.3-<span style="color: #000000;"></span>.fc15.i686.img</pre>
      </td>
    </tr>
  </table>
</div>

Após realizado a alteração e reiniciado o computador, teremos algo assim na saída do _ifconfig_:

<div class="wp_codebox">
  <table>
    <tr id="p770123">
      <td class="code" id="p770code123">
        <pre class="bash" style="font-family:monospace;">eth0      Link encap:Ethernet  Endereço de HW 08:00:<span style="color: #000000;">27</span>:<span style="color: #000000;">34</span>:8E:3F  
          inet end.: 192.168.254.65  Bcast:192.168.254.255  Masc:255.255.255.0
          endereço inet6: fe80::a00:27ff:fe34:8e3f<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">64</span> Escopo:Link
          UP BROADCASTRUNNING MULTICAST  MTU:<span style="color: #000000;">1500</span>  Métrica:<span style="color: #000000;">1</span>
          RX packets:<span style="color: #000000;">453</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> frame:<span style="color: #000000;"></span>
          TX packets:<span style="color: #000000;">83</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> carrier:<span style="color: #000000;"></span>
          colisões:<span style="color: #000000;"></span> txqueuelen:<span style="color: #000000;">1000</span> 
          RX bytes:<span style="color: #000000;">40089</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">39.1</span> KiB<span style="color: #7a0874; font-weight: bold;">&#41;</span>  TX bytes:<span style="color: #000000;">11760</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">11.4</span> KiB<span style="color: #7a0874; font-weight: bold;">&#41;</span>
&nbsp;
lo        Link encap:Loopback Local  
          inet end.: 127.0.0.1  Masc:255.0.0.0
          endereço inet6: ::<span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">128</span> Escopo:Máquina
          UP LOOPBACKRUNNING  MTU:<span style="color: #000000;">16436</span>  Métrica:<span style="color: #000000;">1</span>
          RX packets:<span style="color: #000000;">4</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> frame:<span style="color: #000000;"></span>
          TX packets:<span style="color: #000000;">4</span> errors:<span style="color: #000000;"></span> dropped:<span style="color: #000000;"></span> overruns:<span style="color: #000000;"></span> carrier:<span style="color: #000000;"></span>
          colisões:<span style="color: #000000;"></span> txqueuelen:<span style="color: #000000;"></span> 
          RX bytes:<span style="color: #000000;">240</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">240.0</span> b<span style="color: #7a0874; font-weight: bold;">&#41;</span>  TX bytes:<span style="color: #000000;">240</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">240.0</span> b<span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

Note no exemplo acima que a **eth0** volta a aparecer como antes.