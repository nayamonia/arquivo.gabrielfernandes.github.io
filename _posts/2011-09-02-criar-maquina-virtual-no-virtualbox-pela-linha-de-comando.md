---
id: 224
title: Criar máquina virtual no VirtualBox pelo shell (linha de comando)
date: 2011-09-02T00:07:03+00:00
author: Gabriel Fernandes
layout: post
guid: http://cd2.com.br/?p=224
permalink: /2011/09/02/criar-maquina-virtual-no-virtualbox-pela-linha-de-comando/
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
  - virtualização
tags:
  - linha de comando
  - linux
  - máquina virtual
  - shell
  - virtualbox
---
No pacote padrão do **VirtualBox** há um gerenciador de máquinas virtuais para a linha de comando, chamado _VBoxManage_, capaz de realizar todas as configurações necessárias para criar e gerenciar máquinas virtuais pelo shell.

Vamos ver como podemos criar uma nova **máquina virtual**, preparar para iniciar a instalação do sistema operacional _Windows XP_ a partir de uma _ISO_ do cdrom de instalação e executa-lá no **VirtualBox** utilizando apenas comandos direto na console do Linux.

<!--more [CONTINUAR LENDO]-->


  
A **máquina virtual** sugerida neste post tem o nome _XP Desktop_, siga os passos a seguir para criá-la:

  * Crie a **máquina virtual** com o comando _createvm_ do _VBoxManage_:
<div class="wp_codebox">
  <table>
    <tr id="p224110">
      <td class="code" id="p224code110">
        <pre class="bash" style="font-family:monospace;">VBoxManage createvm <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--ostype</span> WindowsXP <span style="color: #660033;">--register</span></pre>
      </td>
    </tr>
  </table>
</div>

  * Com o comando _modifyvm_ podemos modificar a configuração da **máquina virtual**. O comando abaixo modifica a **máquina virtual** _XP Desktop_ conforme a tabela seguinte:


<div class="wp_codebox">
  <table>
    <tr id="p224111">
      <td class="code" id="p224code111">
        <pre class="bash" style="font-family:monospace;">VBoxManage modifyvm <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--memory</span> <span style="color: #000000;">512</span> <span style="color: #660033;">--vram</span> <span style="color: #000000;">64</span> <span style="color: #660033;">--acpi</span> on <span style="color: #660033;">--boot1</span> dvd <span style="color: #660033;">--nic1</span> bridged <span style="color: #660033;">--bridgeadapter1</span> eth0 <span style="color: #660033;">--vrde</span> on <span style="color: #660033;">--usb</span> on <span style="color: #660033;">--usbehci</span> on</pre>
      </td>
    </tr>
  </table>
</div>

<table border=1> 

</table> 



  * Para criar o disco virtual, podemos usar o _createhd_: 
<div class="wp_codebox">
  <table>
    <tr id="p224112">
      <td class="code" id="p224code112">
        <pre class="bash" style="font-family:monospace;">VBoxManage createhd <span style="color: #660033;">--filename</span> ~<span style="color: #000000; font-weight: bold;">/</span>winxp-20gb.vdi <span style="color: #660033;">-size</span> <span style="color: #000000;">20000</span></pre>
      </td>
    </tr>
  </table>
</div>



O comando acima cria o disco virtual na pasta _home_ do usuário logado, entretanto é permitido usar qualquer local com espaço suficiente para o tamanho do arquivo de disco virtual.

  * Para adicionar controladoras de disco, há o comando _storagectl_, no exemplo abaixo será adicionada na **máquina virtual** uma controladora IDE.
<div class="wp_codebox">
  <table>
    <tr id="p224113">
      <td class="code" id="p224code113">
        <pre class="bash" style="font-family:monospace;">VBoxManage storagectl <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"IDE Controller"</span> <span style="color: #660033;">--add</span> ide</pre>
      </td>
    </tr>
  </table>
</div>

  * Para anexar um disco virtual previamente criado a uma controladora de disco da **máquina virtual**, podemos usar o _storageattach_. Acompanhe o exemplo abaixo e a tabela dos parâmetros a seguir:
<div class="wp_codebox">
  <table>
    <tr id="p224114">
      <td class="code" id="p224code114">
        <pre class="bash" style="font-family:monospace;">VBoxManage storageattach <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--storagectl</span> <span style="color: #ff0000;">"IDE Controller"</span> <span style="color: #660033;">--port</span> <span style="color: #000000;"></span> <span style="color: #660033;">--device</span> <span style="color: #000000;"></span> <span style="color: #660033;">--type</span> hdd <span style="color: #660033;">--medium</span> ~<span style="color: #000000; font-weight: bold;">/</span>winxp-20gb.vdi</pre>
      </td>
    </tr>
  </table>
</div><table border=1> 

</strong>

</table> 

  * O comando para anexar uma imagem ISO a uma controladora de disco da **máquina virtual**, é muito similar ao para adicionar um disco rígido. Observe o exemplo que segue, nele anexamos na **máquina virtual** o ISO _xp.iso_:
<div class="wp_codebox">
  <table>
    <tr id="p224115">
      <td class="code" id="p224code115">
        <pre class="bash" style="font-family:monospace;">VBoxManage storageattach <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--storagectl</span> <span style="color: #ff0000;">"IDE Controller"</span> <span style="color: #660033;">--port</span> <span style="color: #000000;"></span> <span style="color: #660033;">--device</span> <span style="color: #000000;">1</span> <span style="color: #660033;">--type</span> dvddrive <span style="color: #660033;">--medium</span> ~<span style="color: #000000; font-weight: bold;">/</span>xp.iso</pre>
      </td>
    </tr>
  </table>
</div>

  * Um recurso bastante interessante é o compartilhamento de arquivos entre o sistema instalado na **máquina virtual** e o sistema anfitrião. Para compartilhar uma pasta devemos usar o comando _sharedfolder_. No exemplo, cria-se um compartilhamento de nome _temp_ que aponta para a pasta _/tmp_:
<div class="wp_codebox">
  <table>
    <tr id="p224116">
      <td class="code" id="p224code116">
        <pre class="bash" style="font-family:monospace;">VBoxManage sharedfolder add <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #660033;">--name</span> <span style="color: #ff0000;">"temp"</span> <span style="color: #660033;">--hostpath</span> <span style="color: #000000; font-weight: bold;">/</span>tmp<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

Após instalado o sistema operacional na **máquina virtual** é possível acessar este compartilhamento _SMB_. Para acessar, inicie o sistema operacional _Windows XP_ convidado recém-instalado e vá para o endereço _\\VBOXSVR\_ no _Windows Explorer_.

  * Com estas configurações mostradas, já teremos uma **máquina virtual** pronta para ser executada. Para iniciar a **máquina virtual** em segundo plano com acesso por _RDP_ (_Remote Desktop Protocol_, mesmo que o _Microsoft Terminal Services_), execute o comando _VBoxHeadless_ como segue:
<div class="wp_codebox">
  <table>
    <tr id="p224117">
      <td class="code" id="p224code117">
        <pre class="bash" style="font-family:monospace;">VBoxHeadless <span style="color: #660033;">-s</span> <span style="color: #ff0000;">"XP Desktop"</span> <span style="color: #000000; font-weight: bold;">&</span></pre>
      </td>
    </tr>
  </table>
</div>

Enquanto a **máquina virtual** estiver executando, é possível acessar a sua console usando qualquer cliente _Microsoft Terminal Services_ (_RDP_) apontando para o endereço de _IP_ do sistema anfitrião.