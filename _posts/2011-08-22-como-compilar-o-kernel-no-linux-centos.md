---
id: 494
title: Como compilar o Kernel no Linux CentOS
date: 2011-08-22T00:40:00+00:00
author: Gabriel Fernandes
layout: post
guid: http://cd2.com.br/?p=494
permalink: /2011/08/22/como-compilar-o-kernel-no-linux-centos/
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
  - imagens
  - linux
  - shell
  - virtualização
tags:
  - centos
  - kernel
  - linux
---
Este post demonstra como instalar as dependências, baixar as fontes, alterar uma configuração, compilar e instalar um novo **Kernel** **Linux** em uma instalação básica do **CentOS** 5 em modo Server. 

A alteração que vamos fazer no **Kernel**, na prática não irá afetar praticamente nada nele, pois apenas insere no texto de release um valor personalizado. Entretanto é uma alteração simples e segura para ser usada num tutorial que dedica-se a mostrar como é realizada a compilação do **Kernel**.

Para saber mais sobre a instalação do CentOS utilizada neste, visite o post [Instalar Linux CentOS pela rede (ISO netinstall) no Oracle VirtualBox](http://cd2.com.br/2011/08/21/instalar-linux-centos-pela-rede-netinstall-no-oracle-virtualbox/). 

<!--more [CONTINUAR LENDO]-->


  
Vejamos:

  * É preciso certificar-se que algumas dependências estão resolvidas, caso não estejam o comando _yum_ irá resolver e instala-las automaticamente:
<div class="wp_codebox">
  <table>
    <tr id="p49487">
      <td class="code" id="p494code87">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> <span style="color: #c20cb9; font-weight: bold;">gcc</span> ncurses-devel rpm-build <span style="color: #c20cb9; font-weight: bold;">make</span> <span style="color: #c20cb9; font-weight: bold;">wget</span></pre>
      </td>
    </tr>
  </table>
</div>

  * Obtenha os fontes da versão do **Kernel** que deseja compilar. Podemos baixar diretamente na console usando o _wget_, a versão você pode escolher no endereço <a href="http://www.kernel.org/pub/linux/kernel/v2.6/" target="_blank">http://www.kernel.org/pub/linux/kernel/v2.6/</a> ou usar a sugerida no post. Execute:
<del datetime="2012-06-21T20:21:05+00:00">wget http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.19.1.tar.bz2</del>

<div class="wp_codebox">
  <table>
    <tr id="p49488">
      <td class="code" id="p494code88">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">wget</span> http:<span style="color: #000000; font-weight: bold;">//</span>www.kernel.org<span style="color: #000000; font-weight: bold;">/</span>pub<span style="color: #000000; font-weight: bold;">/</span>linux<span style="color: #000000; font-weight: bold;">/</span>kernel<span style="color: #000000; font-weight: bold;">/</span>v2.6<span style="color: #000000; font-weight: bold;">/</span>linux-2.6.19.tar.bz2</pre>
      </td>
    </tr>
  </table>
</div>

  * Desempacote o arquivo das fontes do **Kernel** na pasta _/usr/src_ com o comando:
<del datetime="2012-06-21T20:21:05+00:00">tar jxvf linux-2.6.19.1.tar.bz2 -C /usr/src</del>

<div class="wp_codebox">
  <table>
    <tr id="p49489">
      <td class="code" id="p494code89">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">tar</span> jxvf linux-2.6.19.tar.bz2 <span style="color: #660033;">-C</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src</pre>
      </td>
    </tr>
  </table>
</div>

  * Para se compilar o **Kernel** por padrão utiliza-se o local _/usr/src/linux_, ao desempacotar as fontes é criado uma pasta em _/usr/src_ com o nome e versão do **Kernel**, para que futuramente você possa compilar outras versões dele a melhor maneira de resolver isto é criar um link simbólico. Para a versão de **Kernel** sugerida pelo post segue o comando para este criar o link simbólico:
<del datetime="2012-06-21T20:21:05+00:00">ln -sf /usr/src/linux-2.6.19.1/ /usr/src/linux</del>

<div class="wp_codebox">
  <table>
    <tr id="p49490">
      <td class="code" id="p494code90">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ln</span> <span style="color: #660033;">-sf</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span>linux-2.6.19<span style="color: #000000; font-weight: bold;">/</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span>linux</pre>
      </td>
    </tr>
  </table>
</div>

Entre na pasta das fontes:

<div class="wp_codebox">
  <table>
    <tr id="p49491">
      <td class="code" id="p494code91">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">cd</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span>linux</pre>
      </td>
    </tr>
  </table>
</div>

  * Certifique-se que não há &#8220;sujeira&#8221; de tentativas anteriores de compilação do **Kernel** com o comando:
<div class="wp_codebox">
  <table>
    <tr id="p49492">
      <td class="code" id="p494code92">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">make</span> clean <span style="color: #000000; font-weight: bold;">&&</span> <span style="color: #c20cb9; font-weight: bold;">make</span> mrproper</pre>
      </td>
    </tr>
  </table>
</div>

  * Copie as configurações do **Kernel** atual para a pasta _/usr/src/linux_:
<div class="wp_codebox">
  <table>
    <tr id="p49493">
      <td class="code" id="p494code93">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">cp</span> <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #000000; font-weight: bold;">/</span>config-<span style="color: #000000; font-weight: bold;">&</span><span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #c20cb9; font-weight: bold;">uname</span> -r<span style="color: #7a0874; font-weight: bold;">&#41;</span> .config</pre>
      </td>
    </tr>
  </table>
</div>

  * Proceda com a configuração do **Kernel**, o comando _make menuconfig_ abrirá uma tela para que você personalize seu **Kernel** antes de compilar. Para demonstrar o funcionamento dele vamos apenas alterar o parâmetro _Local version_, este parâmetro inclui um texto personalizado no release do **Kernel**. Coloque a informação que desejar ou siga com a sugestão do post:
<div class="wp_codebox">
  <table>
    <tr id="p49494">
      <td class="code" id="p494code94">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">make</span> menuconfig</pre>
      </td>
    </tr>
  </table>
</div>

Na tela principal da configuração do **Kernel**, use as setas para selecionar _General Setup_ e pressione _[Enter]_:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6077/6067108939_11b39e8d84_z.jpg?resize=640%2C359&#038;ssl=1" alt="make menuconfig 1" width="640" height="359" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108939/)

Em _General Setup_ selecione _Local version &#8211; append to kernel release_:

[<img src="https://i0.wp.com/farm7.staticflickr.com/6187/6067108933_abc8013587_z.jpg?resize=640%2C358&#038;ssl=1" alt="make menuconfig 2" width="640" height="358" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108933/)

Digite o texto a ser adicionado e pressione _[TAB]_ para selecionar _< Ok >_:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6076/6067108929_86db76e1e6_z.jpg?resize=640%2C360&#038;ssl=1" alt="make menuconfig 3" width="640" height="360" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108929/)

Observe entre parênteses o texto adicionado no parâmetro e selecione _< Exit >_ usando o _[TAB]_:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6087/6067108927_b85f855a26_z.jpg?resize=640%2C359&#038;ssl=1" alt="make menuconfig 4" width="640" height="359" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108927/)

Para encerrar a configuração, selecione _< Exit >_ novamente:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6088/6067108925_21cbcb7312_z.jpg?resize=640%2C360&#038;ssl=1" alt="make menuconfig 5" width="640" height="360" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108925/)

Sempre ao sair das configurações do **Kernel** você vai precisar confirmar se quer ou não salvar as alterações. Selecione _< Yes >_ para salvar:

[<img src="https://i0.wp.com/farm7.staticflickr.com/6067/6067108921_7082226630_z.jpg?resize=640%2C359&#038;ssl=1" alt="make menuconfig 6" width="640" height="359" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6067108921/)

  * Compile e construa o pacote RPM, isto pode demorar vários minutos:
<div class="wp_codebox">
  <table>
    <tr id="p49495">
      <td class="code" id="p494code95">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">make</span> rpm</pre>
      </td>
    </tr>
  </table>
</div>

Aguarde o fim da compilação e empacotamento, no final dentre as ultimas mensagens haverá uma que indica onde o pacote RPM foi gerado. A mensagem será parecia com esta:

<div class="wp_codebox">
  <table>
    <tr id="p49496">
      <td class="code" id="p494code96">
        <pre class="bash" style="font-family:monospace;">...
Gravou: <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span>redhat<span style="color: #000000; font-weight: bold;">/</span>RPMS<span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>kernel-2.6.19.1composta.el5-<span style="color: #000000;">1</span>.i386.rpm
...</pre>
      </td>
    </tr>
  </table>
</div>

  * Instale o novo **Kernel** a partir do pacote criado:
<div class="wp_codebox">
  <table>
    <tr id="p49497">
      <td class="code" id="p494code97">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-ivh</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span>redhat<span style="color: #000000; font-weight: bold;">/</span>RPMS<span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>kernel-2.6.19.1composta.el5-<span style="color: #000000;">1</span>.i386.rpm</pre>
      </td>
    </tr>
  </table>
</div>

  * Crie a imagem initrd para a nova versão do **Kernel**:
<div class="wp_codebox">
  <table>
    <tr id="p49498">
      <td class="code" id="p494code98">
        <pre class="bash" style="font-family:monospace;">mkinitrd <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.19.1-composta.el5.img 2.6.19.1-composta.el5</pre>
      </td>
    </tr>
  </table>
</div>

  * Para finalizar a compilação e instalação do novo **Kernel**, precisamos informar ao gerenciador de boot que ele deverá iniciar o sistema com o novo **Kernel**. No **CentOS** o gerenciador padrão é o _Grub_, para adicionar esta opção e colocá-la como padrão de boot, comece com o comando abaixo:
<div class="wp_codebox">
  <table>
    <tr id="p49499">
      <td class="code" id="p494code99">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>grub.conf</pre>
      </td>
    </tr>
  </table>
</div>

Neste arquivo vamos inserir as informações do novo **Kernel**, para fazer esta alteração sem problema, primeiro repita toda a configuração de boot do **Kernel** atual e depois apenas altere a versão nas linhas da configuração do bloco de cima:

Exemplo de arquivo antes de qualquer alteração:

<div class="wp_codebox">
  <table>
    <tr id="p494100">
      <td class="code" id="p494code100">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># grub.conf generated by anaconda</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Note that you do not have to rerun grub after making changes to this file</span>
<span style="color: #666666; font-style: italic;"># NOTICE:  You have a /boot partition.  This means that</span>
<span style="color: #666666; font-style: italic;">#          all kernel and initrd paths are relative to /boot/, eg.</span>
<span style="color: #666666; font-style: italic;">#          root (hd0,0)</span>
<span style="color: #666666; font-style: italic;">#          kernel /vmlinuz-version ro root=/dev/VolGroup00/LogVol00</span>
<span style="color: #666666; font-style: italic;">#          initrd /initrd-version.img</span>
<span style="color: #666666; font-style: italic;">#boot=/dev/sda</span>
<span style="color: #007800;">default</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">timeout</span>=<span style="color: #000000;">5</span>
<span style="color: #007800;">splashimage</span>=<span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>splash.xpm.gz
hiddenmenu
title CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.18-<span style="color: #000000;">238</span>.el5<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.18-<span style="color: #000000;">238</span>.el5 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.18-<span style="color: #000000;">238</span>.el5.img</pre>
      </td>
    </tr>
  </table>
</div>

Repetimos o bloco de configuração de boot do **Kernel** atual:

<div class="wp_codebox">
  <table>
    <tr id="p494101">
      <td class="code" id="p494code101">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># grub.conf generated by anaconda</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Note that you do not have to rerun grub after making changes to this file</span>
<span style="color: #666666; font-style: italic;"># NOTICE:  You have a /boot partition.  This means that</span>
<span style="color: #666666; font-style: italic;">#          all kernel and initrd paths are relative to /boot/, eg.</span>
<span style="color: #666666; font-style: italic;">#          root (hd0,0)</span>
<span style="color: #666666; font-style: italic;">#          kernel /vmlinuz-version ro root=/dev/VolGroup00/LogVol00</span>
<span style="color: #666666; font-style: italic;">#          initrd /initrd-version.img</span>
<span style="color: #666666; font-style: italic;">#boot=/dev/sda</span>
<span style="color: #007800;">default</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">timeout</span>=<span style="color: #000000;">5</span>
<span style="color: #007800;">splashimage</span>=<span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>splash.xpm.gz
hiddenmenu
title CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.18-<span style="color: #000000;">238</span>.el5<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.18-<span style="color: #000000;">238</span>.el5 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.18-<span style="color: #000000;">238</span>.el5.img
&nbsp;
title CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.18-<span style="color: #000000;">238</span>.el5<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.18-<span style="color: #000000;">238</span>.el5 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.18-<span style="color: #000000;">238</span>.el5.img</pre>
      </td>
    </tr>
  </table>
</div>

Altere apenas a versão no primeiro bloco de configuração:

<div class="wp_codebox">
  <table>
    <tr id="p494102">
      <td class="code" id="p494code102">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># grub.conf generated by anaconda</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Note that you do not have to rerun grub after making changes to this file</span>
<span style="color: #666666; font-style: italic;"># NOTICE:  You have a /boot partition.  This means that</span>
<span style="color: #666666; font-style: italic;">#          all kernel and initrd paths are relative to /boot/, eg.</span>
<span style="color: #666666; font-style: italic;">#          root (hd0,0)</span>
<span style="color: #666666; font-style: italic;">#          kernel /vmlinuz-version ro root=/dev/VolGroup00/LogVol00</span>
<span style="color: #666666; font-style: italic;">#          initrd /initrd-version.img</span>
<span style="color: #666666; font-style: italic;">#boot=/dev/sda</span>
<span style="color: #007800;">default</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">timeout</span>=<span style="color: #000000;">5</span>
<span style="color: #007800;">splashimage</span>=<span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>splash.xpm.gz
hiddenmenu
title CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.19.1-composta.el5<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.19.1-composta.el5 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.19.1-composta.el5.img
&nbsp;
title CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>2.6.18-<span style="color: #000000;">238</span>.el5<span style="color: #7a0874; font-weight: bold;">&#41;</span>
        root <span style="color: #7a0874; font-weight: bold;">&#40;</span>hd0,<span style="color: #000000;"></span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
        kernel <span style="color: #000000; font-weight: bold;">/</span>vmlinuz-2.6.18-<span style="color: #000000;">238</span>.el5 ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 rhgb quiet
        initrd <span style="color: #000000; font-weight: bold;">/</span>initrd-2.6.18-<span style="color: #000000;">238</span>.el5.img</pre>
      </td>
    </tr>
  </table>
</div>

  * Reinicie o computador, se tudo correu bem o novo Kernel será inicializado:
<div class="wp_codebox">
  <table>
    <tr id="p494103">
      <td class="code" id="p494code103">
        <pre class="bash" style="font-family:monospace;">init <span style="color: #000000;">6</span></pre>
      </td>
    </tr>
  </table>
</div>

> **[Vídeo Post: clique para assistir ao vídeo que testa este tutorial.](http://cd2.com.br/2011/08/30/video-post-como-compilar-o-kernel-no-linux-centos/)**

> <a href="http://www.formspring.me/nayamonia" target="_blank"><strong>Post Resposta:</strong> Para <strong>tlkundrotas</strong> a partir do <strong>Formspring</strong>. Obrigado.<br /> Sugira um post ou pergunte-me algo você também.</a>