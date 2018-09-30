---
id: 1829
title: Como exportar máquina virtual por linha de comando no VirtualBox
date: 2014-03-12T14:54:38+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1829
permalink: /2014/03/12/como-exportar-maquina-virtual-por-linha-de-comando-no-virtualbox/
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
image: /wp-content/uploads/2014/03/VirtualBox-InterfaceGrafica1.png
categories:
  - virtualização
tags:
  - exportar
  - linha de comando
  - máquina virtual
  - virtualbox
---
O <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a> utiliza o padrão industrial Open Virtualization Format (OVF) para exportar máquinas virtuais.

> O OVF é um padrão aceito na maioria das soluções de virtualização e é normatizado pela Distributed Management Task Force (DMTF). Para mais informações sobre o OVF clique <a title="Site em inglês com a especificação do OVF" href="http://www.dmtf.org/standards/ovf" target="_blank">aqui</a>

Vamos utilizar o subcomando **export** do **VBoxManage** para exportar uma <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> por linha de comando no <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a>, acompanhe comigo:<!--more [CONTINUAR LENDO]-->

  * Para exportar usamos o comando **VBoxManage**, seguido do subcomando **export**, **nome da máquina** e nome do arquivo final (OVF). A sintaxe básica do comando é: <div class="wp_codebox">
      <table>
        <tr id="p1829263">
          <td class="code" id="p1829code263">
            <pre class="bash" style="font-family:monospace;">VBoxManage <span style="color: #7a0874; font-weight: bold;">export</span> <span style="color: #ff0000;">"Nome da maquina no VirtualBox"</span> <span style="color: #660033;">-o</span> Maquina_Virtual_Exportada_em_OVF.ova</pre>
          </td>
        </tr>
      </table>
    </div><figure id="attachment_1813" style="width: 778px" class="wp-caption aligncenter">

[<img src="https://i1.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/VirtualBox-InterfaceGrafica1.png?resize=660%2C461" alt="VirtualBox - Interface Gráfica" width="660" height="461" class="size-full wp-image-1813" srcset="https://i1.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/VirtualBox-InterfaceGrafica1.png?w=778 778w, https://i1.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/VirtualBox-InterfaceGrafica1.png?resize=300%2C209 300w" sizes="(max-width: 660px) 100vw, 660px" data-recalc-dims="1" />](https://i1.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/VirtualBox-InterfaceGrafica1.png)<figcaption class="wp-caption-text">Interface Gráfica do <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a> com a <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> desktop_xp destacada.</figcaption></figure> 

  * Eu tenho aqui no meu <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a> uma <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> com Windows XP instalado chamada **desktop_XP** (veja na imagem acima) que vou utilizar para mostrar um exemplo prático. Basicamente vamos exportar a <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> com o nome **desktop_XP** para o arquivo **exp\_desktop\_XP.ova**, veja como: <div class="wp_codebox">
      <table>
        <tr id="p1829264">
          <td class="code" id="p1829code264">
            <pre class="bash" style="font-family:monospace;">$ VBoxManage <span style="color: #7a0874; font-weight: bold;">export</span> <span style="color: #ff0000;">"desktop_XP"</span> <span style="color: #660033;">-o</span> exp_desktop_XP.ova</pre>
          </td>
        </tr>
      </table>
    </div>



  * Na tela durante a exportação você poderá acompanhar o andamento do **export**, algo parecido com o exemplo abaixo será mostrado: <div class="wp_codebox">
      <table>
        <tr id="p1829265">
          <td class="code" id="p1829code265">
            <pre class="bash" style="font-family:monospace;"><span style="color: #000000;"></span><span style="color: #000000; font-weight: bold;">%</span>...10<span style="color: #000000; font-weight: bold;">%</span>...20<span style="color: #000000; font-weight: bold;">%</span>...30<span style="color: #000000; font-weight: bold;">%</span>...40<span style="color: #000000; font-weight: bold;">%</span>...50<span style="color: #000000; font-weight: bold;">%</span>...60<span style="color: #000000; font-weight: bold;">%</span>...70<span style="color: #000000; font-weight: bold;">%</span>...80<span style="color: #000000; font-weight: bold;">%</span>...90<span style="color: #000000; font-weight: bold;">%</span>...100<span style="color: #000000; font-weight: bold;">%</span>
Successfully exported <span style="color: #000000;">1</span> machine<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>.</pre>
          </td>
        </tr>
      </table>
    </div>

> Atenção para o espaço em disco ao exportar, pois o arquivo OVF final inclui uma cópia do disco virtual existente na <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> comprimido no formato VMDK, independente do formato original do disco, portanto certifique-se de que haverá espaço suficiente no local de destino do arquivo OVF antes de executar o comando.