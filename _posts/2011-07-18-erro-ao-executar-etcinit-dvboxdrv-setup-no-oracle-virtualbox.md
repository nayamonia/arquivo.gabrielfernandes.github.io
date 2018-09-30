---
id: 216
title: 'Erro ao executar &#8216;/etc/init.d/vboxdrv setup&#8217; no Oracle VirtualBox'
date: 2011-07-18T19:42:55+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=216
permalink: /2011/07/18/erro-ao-executar-etcinit-dvboxdrv-setup-no-oracle-virtualbox/
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
image: /wp-content/uploads/2011/07/5952425916_53390bea77_o.png
categories:
  - driver
  - imagens
  - linux
  - shell
  - virtualização
tags:
  - /etc/init.d/vboxdrv setup
  - máquina virtual
  - oracle
  - virtualbox
---
Se o seu **Fedora** foi instalado a partir de uma imagem live, provavelmente você não tem os requisitos mínimos para compilar um módulo do Kernel. O problema é que para executar o **Oracle VirtualBox** é necessário ter o módulo **VirtualBox DKMS** na versão atual do Kernel instalado, ou seja, após uma instalação ou atualização do Kernel, sempre teremos que recompilar estes módulos.

Portanto se você acabou de instalar um novo sistema ou uma nova versão de Kernel e ao tentar executar uma máquina virtual no **Oracle VirtualBox** você é surpreendido com uma mensagem de erro similar a estas das imagens abaixo, você terá que rodar o **/etc/init.d/vboxdrv setup** para recompilar o módulo solicitado, mas para isto seu ambiente precisa atender aos requisitos.

<!--more-->

[<img src="https://i2.wp.com/farm7.staticflickr.com/6125/5952425912_5b05614019.jpg?resize=469%2C197&#038;ssl=1" alt="Erro no Virtual Box - Destaque 02" width="469" height="197" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5952425912/)

[<img src="https://i2.wp.com/farm7.staticflickr.com/6143/5952425916_383a17ed53_z.jpg?resize=510%2C352&#038;ssl=1" alt="Erro no Virtual Box - Destaque 01" width="510" height="352" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5952425916/)

Se o ambiente não atender aos requisitos, quando você tentar rodar o comando indicado na mensagem da tela acima (**/etc/init.d/vboxdrv setup**) uma mensagem de erro ao recompilar o módulo deve ser exibida, muito similar a esta que segue:

<div class="wp_codebox">
  <table>
    <tr id="p21627">
      <td class="code" id="p216code27">
        <pre class="bash" style="font-family:monospace;">Stopping VirtualBox kernel modules                         <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Uninstalling old VirtualBox DKMS kernel modules            <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Trying to register the VirtualBox kernel modules using DKMS<span style="color: #7a0874; font-weight: bold;">&#91;</span>FALHOU<span style="color: #7a0874; font-weight: bold;">&#93;</span>
  <span style="color: #7a0874; font-weight: bold;">&#40;</span>Failed, trying without DKMS<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Recompiling VirtualBox kernel modules                      <span style="color: #7a0874; font-weight: bold;">&#91;</span>FALHOU<span style="color: #7a0874; font-weight: bold;">&#93;</span>
  <span style="color: #7a0874; font-weight: bold;">&#40;</span>Look at <span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>log<span style="color: #000000; font-weight: bold;">/</span>vbox-install.log to <span style="color: #c20cb9; font-weight: bold;">find</span> out what went wrong<span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

Para nos certificarmos que teremos o ambiente apto para compilar este módulo, podemos usar o comando abaixo, pois ele instala ou atualiza os pacotes necessário ao nosso objetivo, segue:

<div class="wp_codebox">
  <table>
    <tr id="p21628">
      <td class="code" id="p216code28">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> kernel-devel kernel-headers <span style="color: #c20cb9; font-weight: bold;">gcc</span></pre>
      </td>
    </tr>
  </table>
</div>

Por fim, se tudo deu certo até aqui, podemos rodar tranquilamente o comando para compilar os módulos do **VirtualBox**, como o exemplo abaixo. Observe que ele falha ao tentar registrar, pois o módulo não existia, mas logo após a recompilação é iniciado corretamente:

<div class="wp_codebox">
  <table>
    <tr id="p21629">
      <td class="code" id="p216code29">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>vboxdrv setup
&nbsp;
Stopping VirtualBox kernel modules                         <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Uninstalling old VirtualBox DKMS kernel modules            <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Trying to register the VirtualBox kernel modules using DKMS<span style="color: #7a0874; font-weight: bold;">&#91;</span>FALHOU<span style="color: #7a0874; font-weight: bold;">&#93;</span>
  <span style="color: #7a0874; font-weight: bold;">&#40;</span>Failed, trying without DKMS<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Recompiling VirtualBox kernel modules                      <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Starting VirtualBox kernel modules                         <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>