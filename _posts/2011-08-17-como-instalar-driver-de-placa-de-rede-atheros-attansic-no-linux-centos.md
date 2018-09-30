---
id: 228
title: Como instalar driver de placa de rede Atheros Attansic no Linux CentOS
date: 2011-08-17T12:52:17+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=228
permalink: /2011/08/17/como-instalar-driver-de-placa-de-rede-atheros-attansic-no-linux-centos/
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
  - driver
  - linux
  - rede
tags:
  - atheros
  - centos
  - driver
  - linux
  - rede
---
A instalação padrão do **CentoOS** 5 não possui o módulo do Kernel para a maioria das placas de rede Atheros (Attansic), para **instalar** o **driver** destas placas podemos fazer de duas formas. Segue:

  * Se tiver acesso a internet no **CentOS** (caso tenha alguma outra placa de rede instalada e funcionando no sistema), adicione o repositório **ELRepo.org** e instale o módulo correto. A placa na qual incentivou este post foi a **Atheros AR8132M** e para ela usei o módulo **atl1e**, este módulo de forma genérica fornece suporte as placas Atheros L1E Gigabit Ethernet com interface PCI Express, mas para a sua **Atheros Attansic**, pode ser diferente, as opções são estas:
<!--more [CONTINUAR LENDO]-->

**kmod-atl1e**: Atheros L1E Gigabit Ethernet com interface PCI Express

**kmod-atl1**: Atheros L1 Gigabit Ethernet com interface PCI Express

**kmod-atl2**: Atheros L2 Gigabit Ethernet com interface PCI Express

Para adicionar o repositório ElRepo.org e instalar o módulo siga os passos:

<div class="wp_codebox">
  <table>
    <tr id="p22883">
      <td class="code" id="p228code83">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">--import</span> http:<span style="color: #000000; font-weight: bold;">//</span>elrepo.org<span style="color: #000000; font-weight: bold;">/</span>RPM-GPG-KEY-elrepo.org</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p22884">
      <td class="code" id="p228code84">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-Uvh</span> http:<span style="color: #000000; font-weight: bold;">//</span>elrepo.org<span style="color: #000000; font-weight: bold;">/</span>elrepo-release-<span style="color: #000000;">5</span>-<span style="color: #000000;">3</span>.el5.elrepo.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p22885">
      <td class="code" id="p228code85">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> kmod-atl1e</pre>
      </td>
    </tr>
  </table>
</div>

Aguarde o término da instalação e reinicie o computador.

  * Se o sistema não tiver acesso a internet, baixe o pacote RPM kmod-atl1e (ou outro, conforme modelo da sua placa) do repositório ELRepo.org em outro computador, a versão do arquivo que baixei foi a **kmod-atl1e-1.0.1.14-1.el5.elrepo.i686.rpm**, mas você deve procurar por kmod-atl1e e selecionar o pacote mais recente para baixar. 
    Link para download na versão **32 bits**:
  
    <a href="http://elrepo.reloumirrors.net/elrepo/el5/i386/RPMS/" target="_blank">http://elrepo.reloumirrors.net/elrepo/el5/i386/RPMS/</a>
    
    Link para download na versão **64 bits**:
  
    <a href="http://elrepo.reloumirrors.net/elrepo/el5/x86_64/RPMS/" target="_blank">http://elrepo.reloumirrors.net/elrepo/el5/x86_64/RPMS/</a>
    
    Caso encontre problemas com os links acima, você pode ir para a página padrão de download do **ELRepo.org** (<a href="http://elrepo.org/tiki/Download" target="_blank">ELRepo.org Downloads</a>) e selecionar um dos espelhos, todos apontam para a raiz do repositório e possuem árvore de diretórios semelhante. Os pacotes que precisamos estão na pasta **elrepo/**, que por sua vez possui a pasta da versão do **CentOS**, aqui utilizamos a **el5/**, em seguida a pasta da arquitetura, 32 bits é **i386/** e 64 bits é **x86_64/**, por fim a pasta **RPMS/**.
    
    Instale o modulo **atl1e** a partir do pacote rpm baixado, lembrando que o nome do arquivo que você baixou pode ser diferente:
    
    <div class="wp_codebox">
      <table>
        <tr id="p22886">
          <td class="code" id="p228code86">
            <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-ivh</span> kmod-atl1e-1.0.1.14-<span style="color: #000000;">1</span>.el5.elrepo.i686.rpm</pre>
          </td>
        </tr>
      </table>
    </div>
    
    Aguarde o término da instalação e reinicie o computador.</ul> 
    
    Após o reinício, a placa de rede deverá funcionar perfeitamente.