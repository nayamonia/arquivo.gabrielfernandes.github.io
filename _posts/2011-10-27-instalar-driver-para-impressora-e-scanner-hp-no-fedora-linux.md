---
id: 936
title: Instalar Driver para Impressora e Scanner HP no Fedora Linux
date: 2011-10-27T00:16:41+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=936
permalink: /2011/10/27/instalar-driver-para-impressora-e-scanner-hp-no-fedora-linux/
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
image: /wp-content/uploads/2011/10/hp+impressora+copiadora+laser+e+scanner+multifuncional+m1120+sao+paulo+sp+brasil__335369_1.jpg
categories:
  - driver
  - imagens
  - linux
tags:
  - driver
  - fedora
  - hp
  - impressora
  - linux
  - scanner
---
A Hewlett-Packard (**HP**) fornece uma solução open source* que instala diversos drivers para impressão, digitalização e fax em produtos jato de tinta e impressoras a laser da **HP**. Este projeto é o **HP** Linux Imaging and Printing (_HPLIP_), ele fornece suporte à impressão em 2.053 modelos de impressoras, como Deskjet, Officejet, Photosmart, PSC, Business Inkjet, LaserJet, MFP Edgeline, e LaserJet MFP. 

No **Fedora** podemos **instalar** automaticamente pelo gerenciador de pacotes _YUM_, para instalar use:
  
<!--more [CONTINUAR LENDO]-->

<div class="wp_codebox">
  <table>
    <tr id="p936149">
      <td class="code" id="p936code149">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> hplip</pre>
      </td>
    </tr>
  </table>
</div>

Siga os procedimentos na tela, aguarde o download e término da instalação que os drivers do **hplip** já estarão disponíveis.

Nas imagens observe como estava a lista de drivers de impressoras HP no programa _system-config-printer_ antes e depois da instalação do _hplip_ no meu Desktop:
  
[<img src="https://i0.wp.com/farm7.staticflickr.com/6108/6285154176_1c3fde4e4b_z.jpg?resize=640%2C558&#038;ssl=1" alt="Trecho da lista de drivers de impressoras HP antes de instalar HPLIP" width="640" height="558" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6285154176/)

Após a instalação novos drivers são listados:
  
[<img src="https://i1.wp.com/farm7.staticflickr.com/6033/6285154166_bc2d8b0498_z.jpg?resize=640%2C558&#038;ssl=1" alt="Trecho da lista de drivers de impressoras HP após instalar HPLIP" width="640" height="558" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6285154166/)

* Licenças MIT, BSD e GPL