---
id: 230
title: Como instalar driver da placa de rede Realtek RTL8101E no CentOS
date: 2011-08-07T23:59:06+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=230
permalink: /2011/08/07/como-instalar-driver-da-placa-de-rede-realtek-rtl8101e-no-centos/
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
  - automação
  - driver
  - linux
  - rede
tags:
  - 8101e
  - centos
  - driver
  - linux
  - realtek
  - rede
---
O CentoOS na placa mãe **Gigabyte G31M-S2C** com a rede on-board **Realtek 8101E** sobe o módulo **r1000**, que chega a funcionar, entretanto causa quedas intermitentes na conexão.

Deve-se instalar o módulo correto para esta placa de rede, isto pode ser feito em poucos instantes, segue:

<!--more [CONTINUAR LENDO]-->

  * Se tiver acesso a internet no CentOS, adicione o repositório ELRepo.org e instale o módulo kmod-r8101:
<div class="wp_codebox">
  <table>
    <tr id="p23062">
      <td class="code" id="p230code62">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">--import</span> http:<span style="color: #000000; font-weight: bold;">//</span>elrepo.org<span style="color: #000000; font-weight: bold;">/</span>RPM-GPG-KEY-elrepo.org</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p23063">
      <td class="code" id="p230code63">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-Uvh</span> http:<span style="color: #000000; font-weight: bold;">//</span>elrepo.org<span style="color: #000000; font-weight: bold;">/</span>elrepo-release-<span style="color: #000000;">5</span>-<span style="color: #000000;">3</span>.el5.elrepo.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p23064">
      <td class="code" id="p230code64">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> kmod-r8101</pre>
      </td>
    </tr>
  </table>
</div>

Aguarde o término da instalação e reinicie o computador.

  * Se o sistema não tiver acesso a internet, baixe o pacote RPM kmod-r8101 do repositório ELRepo.org em outro computador, a versão do arquivo que baixei foi a **kmod-r8101-1.019.00_NAPI-2.el5.elrepo.i686.rpm**, mas você deve procurar por kmod-r8101 e selecionar o pacote mais recente para baixar. 
    Link para download na versão **32 bits**:
  
    <a href="http://elrepo.reloumirrors.net/elrepo/el5/i386/RPMS/" target="_blank">http://elrepo.reloumirrors.net/elrepo/el5/i386/RPMS/</a>
    
    Link para download na versão **64 bits**:
  
    <a href="http://elrepo.reloumirrors.net/elrepo/el5/x86_64/RPMS/" target="_blank">http://elrepo.reloumirrors.net/elrepo/el5/x86_64/RPMS/</a>
    
    Caso encontre problemas com os links acima, você pode ir para a página padrão de download do ELRepo.org (<a href="http://elrepo.org/tiki/Download" target="_blank">ELRepo.org Downloads</a>) e selecionar um dos espelhos, todos apontam para a raiz do repositório e possuem árvore de diretórios semelhante. Os pacotes que precisamos estão na pasta **elrepo/**, que por sua vez possui a pasta da versão do **CentOS**, aqui utilizamos a **el5/**, em seguida a pasta da arquitetura, 32 bits é **i386/** e 64 bits é **x86_64/**, por fim a pasta **RPMS/**.
    
    Instale o modulo **r8101** a partir do pacote rpm baixado, lembrando que o nome do arquivo que você baixou pode ser diferente:
    
    <div class="wp_codebox">
      <table>
        <tr id="p23065">
          <td class="code" id="p230code65">
            <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-ivh</span> kmod-r8101-1.019.00_NAPI-<span style="color: #000000;">2</span>.el5.elrepo.i686.rpm</pre>
          </td>
        </tr>
      </table>
    </div>
    
    Aguarde o término da instalação e reinicie o computador.</ul> 
    
    Após o reinício, a placa de rede deverá funcionar perfeitamente.