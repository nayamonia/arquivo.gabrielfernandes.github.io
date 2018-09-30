---
id: 1012
title: 'Como converter &#8211; clonar &#8211; máquina real para virtual no Oracle VirtualBox em Linux'
date: 2011-11-16T17:43:08+00:00
author: Gabriel Fernandes
layout: post
guid: http://cd2.com.br/?p=1012
permalink: /2011/11/16/como-converter-clonar-maquina-real-para-virtual-no-oracle-virtualbox-em-linux/
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
image: /wp-content/uploads/2011/11/virtualbox-4.1.12.png
categories:
  - linux
  - shell
  - virtualização
tags:
  - linux
  - máquina virtual
  - virtualbox
---
**Converter** uma **máquina real para virtual** é uma tarefa bastante interessante e que pode ser relativamente simples.

Podemos fazer isto em uma **máquina real** com um disco rígido sem muitas dores de cabeça, utilizando-se apenas alguns comandos, mas quero chamar atenção para a afirmação &#8220;um disco rígido&#8221;, pois o processo mais crítico desta conversão de **real** para **virtual** é a criação e conversão da imagem do disco rígido **real** para um arquivo de disco **virtual** &#8211; formato VDI &#8211; e se você tiver mais de um disco, ou até mesmo um array de discos em RAID, deve ter bastante atenção neste processo.

<!--more [CONTINUAR LENDO]-->

Acompanhe os comandos necessários para tal feito, segue:

  * Identifique o dispositivo da **máquina real** que possui a instalação do sistema operacional e com o _DD_ crie uma imagem RAW do disco rígido **real**. O comando que segue, usa como exemplo o dispositivo disco rígido **real** o endereço /dev/sdb:
<div class="wp_codebox">
  <table>
    <tr id="p1012150">
      <td class="code" id="p1012code150">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">dd</span> <span style="color: #007800;">if</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>sdb <span style="color: #007800;">of</span>=meu_disco.bin <span style="color: #007800;">bs</span>=<span style="color: #000000;">1024</span></pre>
      </td>
    </tr>
  </table>
</div>

> Vale ressaltar que não é seguro clonar um hd que está em uso, portanto deve-se fazer boot com um sistema Live **Linux** ou colocar o hd em outra instalação **Linux**.

  * Converta do formato RAW (binário) para _VDI_:
<div class="wp_codebox">
  <table>
    <tr id="p1012151">
      <td class="code" id="p1012code151">
        <pre class="bash" style="font-family:monospace;">VBoxManage convertdd meu_disco.bin meu_disco.vdi</pre>
      </td>
    </tr>
  </table>
</div>

  * Compacte o _VDI_:
<div class="wp_codebox">
  <table>
    <tr id="p1012152">
      <td class="code" id="p1012code152">
        <pre class="bash" style="font-family:monospace;">VBoxManage modifyvdi meu_disco.vdi compact</pre>
      </td>
    </tr>
  </table>
</div>

  * Crie uma nova **máquina virtual** no **VirtualBox**. No exemplo estamos criando uma **máquina** com sistema operacional **Linux** com o nome MinhaMaquina, você pode mudar isto:
<div class="wp_codebox">
  <table>
    <tr id="p1012153">
      <td class="code" id="p1012code153">
        <pre class="bash" style="font-family:monospace;">VBoxManage createvm <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--ostype</span> Linux <span style="color: #660033;">--register</span></pre>
      </td>
    </tr>
  </table>
</div>

> Mais informações sobre parâmetros: [Criar **máquina virtual** no **VirtualBox** pelo shell (linha de comando)](http://cd2.com.br/2011/09/02/criar-maquina-virtual-no-virtualbox-pela-linha-de-comando/)

  * Ajuste alguns parâmetros na **máquina** recém criada:
<div class="wp_codebox">
  <table>
    <tr id="p1012154">
      <td class="code" id="p1012code154">
        <pre class="bash" style="font-family:monospace;">VBoxManage modifyvm <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--memory</span> <span style="color: #000000;">512</span> <span style="color: #660033;">--vram</span> <span style="color: #000000;">64</span> <span style="color: #660033;">--acpi</span> on <span style="color: #660033;">--boot1</span> dvd <span style="color: #660033;">--nic1</span> bridged <span style="color: #660033;">--bridgeadapter1</span> eth0 <span style="color: #660033;">--vrde</span> on <span style="color: #660033;">--usb</span> on <span style="color: #660033;">--usbehci</span> on</pre>
      </td>
    </tr>
  </table>
</div>

  * Adicione uma controladora de disco:
IDE:

<div class="wp_codebox">
  <table>
    <tr id="p1012155">
      <td class="code" id="p1012code155">
        <pre class="bash" style="font-family:monospace;">VBoxManage storagectl <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"IDE Controller"</span> <span style="color: #660033;">--add</span> ide</pre>
      </td>
    </tr>
  </table>
</div>

SATA:

<div class="wp_codebox">
  <table>
    <tr id="p1012156">
      <td class="code" id="p1012code156">
        <pre class="bash" style="font-family:monospace;">VBoxManage storagectl <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"SATA Controller"</span> <span style="color: #660033;">--add</span> sata</pre>
      </td>
    </tr>
  </table>
</div>

  * Conecte o disco na controladora recém criada:
IDE:

<div class="wp_codebox">
  <table>
    <tr id="p1012157">
      <td class="code" id="p1012code157">
        <pre class="bash" style="font-family:monospace;">VBoxManage storageattach <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--storagectl</span> <span style="color: #ff0000;">"IDE Controller"</span> <span style="color: #660033;">--port</span> <span style="color: #000000;"></span> <span style="color: #660033;">--device</span> <span style="color: #000000;"></span> <span style="color: #660033;">--type</span> hdd <span style="color: #660033;">--medium</span>  meu_disco.vdi</pre>
      </td>
    </tr>
  </table>
</div>

SATA:

<div class="wp_codebox">
  <table>
    <tr id="p1012158">
      <td class="code" id="p1012code158">
        <pre class="bash" style="font-family:monospace;">VBoxManage storageattach <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #660033;">--storagectl</span> <span style="color: #ff0000;">"SATA Controller"</span> <span style="color: #660033;">--port</span> <span style="color: #000000;"></span> <span style="color: #660033;">--device</span> <span style="color: #000000;"></span> <span style="color: #660033;">--type</span> hdd <span style="color: #660033;">--medium</span>  meu_disco.vdi</pre>
      </td>
    </tr>
  </table>
</div>

  * Inicie a **máquina virtual** com o comando abaixo e acesse por RDP ou abra o Gerenciador de **Máquinas Virtuais** do **VirtualBox** e inicie a mesma:
<div class="wp_codebox">
  <table>
    <tr id="p1012159">
      <td class="code" id="p1012code159">
        <pre class="bash" style="font-family:monospace;">VBoxHeadless <span style="color: #660033;">-s</span> <span style="color: #ff0000;">"MinhaMaquina"</span> <span style="color: #000000; font-weight: bold;">&</span></pre>
      </td>
    </tr>
  </table>
</div>

Se tudo correu bem, não há mais nada a fazer. 

Seja feliz.