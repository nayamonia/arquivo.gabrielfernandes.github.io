---
id: 700
title: Instalar Oracle VM VirtualBox Extension Pack no Linux
date: 2011-08-29T19:35:37+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=700
permalink: /2011/08/29/instalar-oracle-vm-virtualbox-extension-pack-no-linux/
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
image: /wp-content/uploads/2011/08/6094140879_6f772114cf_z.jpg
categories:
  - virtualização
tags:
  - extension pack
  - linux
  - máquina virtual
  - oracle
  - virtualbox
---
Instalar o **Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** é uma tarefa bastante simples e fundamental para que possamos habilitar dispositivos _USB 2.0_, habilitar a _conexão remota via RDP (mesmo protocolo do Windows Terminal Server)_ e também para habilitar o _boot via PXE_ em placas da Intel em uma <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank">máquina virtual</a>.

Esta instalação pode ser realizada com pelo menos dois métodos, um para usuários _avançados_, direto no shell, sem o uso de interface gráfica e outro para usuários _normais_, pela interface gráfica.

<!--more [CONTINUAR LENDO]-->


  
Instalação avançada:

  * Baixe o **<a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> 4.X.X Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** com o comando abaixo:
<div class="wp_codebox">
  <table>
    <tr id="p700105">
      <td class="code" id="p700code105">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">wget</span> http:<span style="color: #000000; font-weight: bold;">//</span>download.virtualbox.org<span style="color: #000000; font-weight: bold;">/</span>virtualbox<span style="color: #000000; font-weight: bold;">/</span>$<span style="color: #7a0874; font-weight: bold;">&#40;</span>VBoxManage <span style="color: #660033;">-v</span> <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #c20cb9; font-weight: bold;">cut</span> <span style="color: #660033;">-d</span> r -f1<span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #000000; font-weight: bold;">/</span>Oracle_VM_VirtualBox_Extension_Pack-$<span style="color: #7a0874; font-weight: bold;">&#40;</span>VBoxManage <span style="color: #660033;">-v</span> <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #c20cb9; font-weight: bold;">cut</span> <span style="color: #660033;">-d</span> r -f1<span style="color: #7a0874; font-weight: bold;">&#41;</span>-$<span style="color: #7a0874; font-weight: bold;">&#40;</span>VBoxManage <span style="color: #660033;">-v</span> <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #c20cb9; font-weight: bold;">cut</span> <span style="color: #660033;">-d</span> r -f2<span style="color: #7a0874; font-weight: bold;">&#41;</span>.vbox-extpack</pre>
      </td>
    </tr>
  </table>
</div>

  * Se você já tiver uma versão do **Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** instalada, mesmo sem estar funcionando, temos que desinstalar para instalar novamente. Para desinstalar use: 
<div class="wp_codebox">
  <table>
    <tr id="p700106">
      <td class="code" id="p700code106">
        <pre class="bash" style="font-family:monospace;">VBoxManage extpack uninstall <span style="color: #ff0000;">"Oracle VM VirtualBox Extension Pack"</span></pre>
      </td>
    </tr>
  </table>
</div>

  * Instale o **Oracle VM VirtualBox Extension Pack**: 
<div class="wp_codebox">
  <table>
    <tr id="p700107">
      <td class="code" id="p700code107">
        <pre class="bash" style="font-family:monospace;">VBoxManage extpack <span style="color: #c20cb9; font-weight: bold;">install</span> Oracle_VM_VirtualBox_Extension_Pack-$<span style="color: #7a0874; font-weight: bold;">&#40;</span>VBoxManage <span style="color: #660033;">-v</span> <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #c20cb9; font-weight: bold;">cut</span> <span style="color: #660033;">-d</span> r -f1<span style="color: #7a0874; font-weight: bold;">&#41;</span>-$<span style="color: #7a0874; font-weight: bold;">&#40;</span>VBoxManage <span style="color: #660033;">-v</span> <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #c20cb9; font-weight: bold;">cut</span> <span style="color: #660033;">-d</span> r -f2<span style="color: #7a0874; font-weight: bold;">&#41;</span>.vbox-extpack</pre>
      </td>
    </tr>
  </table>
</div>

Pronto, se tudo correu bem, o serviço está pronto.

Instalação pela interface gráfica (_Gnome 3_):

  * Baixe o **<a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> 4.X.X Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** da área de download do site do <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a>, acesse este link <a href="http://www.virtualbox.org/wiki/Downloads" target="_blank">http://www.virtualbox.org/wiki/Downloads</a>. Você deve estar em um site como o da figura abaixo, nele há um link para download com o nome _All platforms_ este link aponta para o download do extension pack para a última versão do <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a>, tenha certeza de que está com a última versão instalada ou baixe um pacote mais antigo:
[<img src="https://i2.wp.com/farm7.staticflickr.com/6190/6094140879_6f772114cf_z.jpg?resize=640%2C246&#038;ssl=1" alt="VirtualBox-extensioPack" width="640" height="246" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6094140879/)

  * Depois de baixar, acesse a pasta onde esta o arquivo e de um duplo clique nele, seu **<a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a>** deverá ser iniciado e uma tela como esta que segue deve ser apresentada para você:
[<img src="https://i0.wp.com/farm7.staticflickr.com/6066/6094696476_b2fd0d5146_z.jpg?resize=640%2C207&#038;ssl=1" alt="Captura_de_tela-2" width="640" height="207" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6094696476/)

Clique em instalar e pronto, se tudo correu bem, temos o **Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** instalado. </ul> 

> **Dica 1:** Note que na instalação via terminal, você não precisa preocupar-se com a versão do **<a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a>**, porque o comando automaticamente faz o download do extension pack para a versão instalada. 

> **Dica 2:**Se ao tentar instalar o **<a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> 4.X.X Oracle VM <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank">VirtualBox</a> Extension Pack** retornar um erro similar a este:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p700108">
>       <td class="code" id="p700code108">
>         <pre class="bash" style="font-family:monospace;">VBoxManage: error: Failed to <span style="color: #c20cb9; font-weight: bold;">install</span> <span style="color: #ff0000;">"Oracle_VM_VirtualBox_Extension_Pack-4.1.8-75467.vbox-extpack"</span>: 
The installer failed with <span style="color: #7a0874; font-weight: bold;">exit</span> code <span style="color: #000000;">127</span>: The value <span style="color: #000000; font-weight: bold;">for</span> the SHELL variable 
was not found the <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>shells <span style="color: #c20cb9; font-weight: bold;">file</span>
&nbsp;
VBoxManage: error: This incident has been reported.</pre>
>       </td>
>     </tr>
>   </table>
> </div>
> 
> Adicione ao final do arquivo **/etc/shells** a linha abaixo:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p700109">
>       <td class="code" id="p700code109">
>         <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>bin<span style="color: #000000; font-weight: bold;">/</span><span style="color: #c20cb9; font-weight: bold;">bash</span></pre>
>       </td>
>     </tr>
>   </table>
> </div>
> 
> E execute o comando de instalação novamente.