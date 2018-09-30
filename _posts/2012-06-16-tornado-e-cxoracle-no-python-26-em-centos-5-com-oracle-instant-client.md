---
id: 1447
title: Tornado e cx_Oracle no Python 2.6 em CentOS 5 com Oracle Instant Client
date: 2012-06-16T19:43:11+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1447
permalink: /2012/06/16/tornado-e-cxoracle-no-python-26-em-centos-5-com-oracle-instant-client/
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
image: /wp-content/uploads/2012/06/oraclac.jpg
categories:
  - banco de dados
  - desenvolvimento
  - linux
tags:
  - centos
  - cx_Oracle
  - instant client
  - oracle
  - python
  - tornado
---
A instalação do **CentOS** utilizada para escrever este post é resultado dos procedimentos publicados no post <a href="http://www.gabrielfernandes.org/2012/06/04/como-criar-e-instalar-o-centos-em-uma-maquina-virtual-no-servidor-xen-centos-5/" title="Clique e veja como criar uma VM e instalar o CentOS em um Servidor Xen" target="_blank">Como criar uma máquina virtual no servidor <strong>Xen Centos 5</strong> e instalar o <strong>CentOS</strong> pela rede</a> e a instalação do **Tornado**, bem como do **Python 2.6**, que é pré-requisito para o **Tornado** que não está disponível no **CentOS 5.8** é resultado dos procedimentos publicados no post <a href="http://www.gabrielfernandes.org/2012/06/15/configurar-o-centos-5-para-suporte-ao-tornado-no-python-26/" title="Clique aqui e veja como instalei o Python 2.6 e o Tornado" target="_blank">Configurar o <strong>CentOS</strong> 5 para suporte ao <strong>Tornado</strong> no <strong>Python 2.6</strong></a>, os dois aqui mesmo na [Compostagem Digital](http://cd2.com.br "Se clicar aqui, você voltará para a página principal do blog"), portanto para ter uma instalação como esta de uma conferida neles.

Continuando &#8230;
  
<!--more [CONTINUAR LENDO]-->

Para instalar o **Oracle Instant Client** precisamos baixar os pacotes Basic e SDK do **Oracle Instant Client** da própria **Oracle**, veja como fazer isto neste meu outro post <a href="http://www.gabrielfernandes.org/2011/08/01/como-fazer-php-funcionar-com-oracle-oci8-no-fedora-15/" title="Como baixar o instant client da Oracle" target="_blank">Como configurar o PHP com suporte Oracle no Fedora – extension oci8</a>, não é necessário ler o artigo inteiro, já na primeira parte há a explicação de como proceder para realizar o download do **Oracle Instant Client** e isto é o suficiente para você ter sucesso nesta primeira etapa e depois voltar pra cá, que estou te esperando.

Considerando que tu já fez o download do **Oracle Instant Client**, vou começar a mostrar como instalar e configurar o ambiente.

Comece pela instalação do pacote **Oracle Instant Client** Basic, com o comando abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p1447220">
      <td class="code" id="p1447code220">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> <span style="color: #660033;">--nogpgcheck</span> oracle-instantclient11.2-basic-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm</pre>
      </td>
    </tr>
  </table>
</div>

> O parâmetro _&#8211;nogpgcheck_ ignora a checagem da assinatura do pacote.

Se tudo correu bem, algo como isto abaixo deve estar sendo exibido na sua tela:

<div class="wp_codebox">
  <table>
    <tr id="p1447221">
      <td class="code" id="p1447code221">
        <pre class="bash" style="font-family:monospace;">Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 <span style="color: #000000; font-weight: bold;">*</span> base: centos.pop.com.br
 <span style="color: #000000; font-weight: bold;">*</span> epel: mirror.cogentco.com
 <span style="color: #000000; font-weight: bold;">*</span> extras: centos.pop.com.br
 <span style="color: #000000; font-weight: bold;">*</span> updates: centos.pop.com.br
Setting up Install Process
Examining oracle-instantclient11.2-basic-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm: oracle-instantclient11.2-basic-11.2.0.3.0-<span style="color: #000000;">1</span>.i386
Marking oracle-instantclient11.2-basic-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm to be installed
Resolving Dependencies
--<span style="color: #000000; font-weight: bold;">&gt;</span> Running transaction check
---<span style="color: #000000; font-weight: bold;">&gt;</span> Package oracle-instantclient11.2-basic.i386 <span style="color: #000000;"></span>:11.2.0.3.0-<span style="color: #000000;">1</span> <span style="color: #000000; font-weight: bold;">set</span> to be updated
--<span style="color: #000000; font-weight: bold;">&gt;</span> Finished Dependency Resolution
&nbsp;
Dependencies Resolved
&nbsp;
===========================================================================================================================================
 Package                                Arch         Version               Repository                                                 Size
===========================================================================================================================================
Installing:
 oracle-instantclient11.2-basic         i386         11.2.0.3.0-<span style="color: #000000;">1</span>          <span style="color: #000000; font-weight: bold;">/</span>oracle-instantclient11.2-basic-11.2.0.3.0-<span style="color: #000000;">1</span>.i386         <span style="color: #000000;">168</span> M
&nbsp;
Transaction Summary
===========================================================================================================================================
Install       <span style="color: #000000;">1</span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Upgrade       <span style="color: #000000;"></span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
&nbsp;
Total <span style="color: #c20cb9; font-weight: bold;">size</span>: <span style="color: #000000;">168</span> M
Is this ok <span style="color: #7a0874; font-weight: bold;">&#91;</span>y<span style="color: #000000; font-weight: bold;">/</span>N<span style="color: #7a0874; font-weight: bold;">&#93;</span>: y</pre>
      </td>
    </tr>
  </table>
</div>

Alí em cima pressione &#8220;y&#8221; para instalar.

<div class="wp_codebox">
  <table>
    <tr id="p1447222">
      <td class="code" id="p1447code222">
        <pre class="bash" style="font-family:monospace;">Downloading Packages:
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : oracle-instantclient11.2-basic                                                                                      <span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">1</span> 
&nbsp;
Installed:
  oracle-instantclient11.2-basic.i386 <span style="color: #000000;"></span>:11.2.0.3.0-<span style="color: #000000;">1</span>                                                                                       
&nbsp;
Complete<span style="color: #000000; font-weight: bold;">!</span></pre>
      </td>
    </tr>
  </table>
</div>

Terminado a instalação do pacote **Oracle Instant Client** Basic, repita a operação para instalar o pacote **Oracle Instant Client** Devel,como segue:

<div class="wp_codebox">
  <table>
    <tr id="p1447223">
      <td class="code" id="p1447code223">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> <span style="color: #660033;">--nogpgcheck</span> oracle-instantclient11.2-devel-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Tu deves estar vendo algo assim na console:

<div class="wp_codebox">
  <table>
    <tr id="p1447224">
      <td class="code" id="p1447code224">
        <pre class="bash" style="font-family:monospace;">Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 <span style="color: #000000; font-weight: bold;">*</span> base: mirror.linux.duke.edu
 <span style="color: #000000; font-weight: bold;">*</span> epel: mirror.cogentco.com
 <span style="color: #000000; font-weight: bold;">*</span> extras: centos.netnitco.net
 <span style="color: #000000; font-weight: bold;">*</span> updates: centos.digitalcompass.net
Setting up Install Process
Examining oracle-instantclient11.2-devel-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm: oracle-instantclient11.2-devel-11.2.0.3.0-<span style="color: #000000;">1</span>.i386
Marking oracle-instantclient11.2-devel-11.2.0.3.0-<span style="color: #000000;">1</span>.i386.rpm to be installed
Resolving Dependencies
--<span style="color: #000000; font-weight: bold;">&gt;</span> Running transaction check
---<span style="color: #000000; font-weight: bold;">&gt;</span> Package oracle-instantclient11.2-devel.i386 <span style="color: #000000;"></span>:11.2.0.3.0-<span style="color: #000000;">1</span> <span style="color: #000000; font-weight: bold;">set</span> to be updated
--<span style="color: #000000; font-weight: bold;">&gt;</span> Finished Dependency Resolution
&nbsp;
Dependencies Resolved
&nbsp;
===========================================================================================================================================
 Package                                Arch         Version               Repository                                                 Size
===========================================================================================================================================
Installing:
 oracle-instantclient11.2-devel         i386         11.2.0.3.0-<span style="color: #000000;">1</span>          <span style="color: #000000; font-weight: bold;">/</span>oracle-instantclient11.2-devel-11.2.0.3.0-<span style="color: #000000;">1</span>.i386         <span style="color: #000000;">1.9</span> M
&nbsp;
Transaction Summary
===========================================================================================================================================
Install       <span style="color: #000000;">1</span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Upgrade       <span style="color: #000000;"></span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
&nbsp;
Total <span style="color: #c20cb9; font-weight: bold;">size</span>: <span style="color: #000000;">1.9</span> M
Is this ok <span style="color: #7a0874; font-weight: bold;">&#91;</span>y<span style="color: #000000; font-weight: bold;">/</span>N<span style="color: #7a0874; font-weight: bold;">&#93;</span>: y</pre>
      </td>
    </tr>
  </table>
</div>

De novo pressione o &#8220;y&#8221; para instalar e aguarde o término da instalação:

<div class="wp_codebox">
  <table>
    <tr id="p1447225">
      <td class="code" id="p1447code225">
        <pre class="bash" style="font-family:monospace;">Downloading Packages:
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : oracle-instantclient11.2-devel                                                                                      <span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">1</span> 
&nbsp;
Installed:
  oracle-instantclient11.2-devel.i386 <span style="color: #000000;"></span>:11.2.0.3.0-<span style="color: #000000;">1</span>                                                                                       
&nbsp;
Complete<span style="color: #000000; font-weight: bold;">!</span></pre>
      </td>
    </tr>
  </table>
</div>

Legal, simples até agora, não se preocupe, vai continuar sendo simples, siga em frente.

Crie um arquivo com o nome _instantclient.sh_ e coloque o conteúdo que segue abaixo nele:

<div class="wp_codebox">
  <table>
    <tr id="p1447226">
      <td class="code" id="p1447code226">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;">#!/bin/bash</span>
<span style="color: #7a0874; font-weight: bold;">export</span> <span style="color: #007800;">ORACLE_HOME</span>=<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">11.2</span><span style="color: #000000; font-weight: bold;">/</span>client
<span style="color: #7a0874; font-weight: bold;">export</span> <span style="color: #007800;">LD_LIBRARY_PATH</span>=<span style="color: #007800;">$LD_LIBRARY_PATH</span>:<span style="color: #007800;">$ORACLE_HOME</span></pre>
      </td>
    </tr>
  </table>
</div>

> É bom saber que o ORACLE_HOME pode ser diferente, pois tu podes ter instalado outra versão do **Oracle Instant Client**, para saber qual o caminho correto da tua instalação, use o famigerado comando _ls_ como segue:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p1447227">
>       <td class="code" id="p1447code227">
>         <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ls</span> <span style="color: #660033;">-l</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle</pre>
>       </td>
>     </tr>
>   </table>
> </div>

Agora copie este arquivo _instantclient.sh_ para a pasta _/etc/profile.d_:

<div class="wp_codebox">
  <table>
    <tr id="p1447228">
      <td class="code" id="p1447code228">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">cp</span> instantclient.sh <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>profile.d<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

> O que fizemos ai em cima é para dizer ao sistema operacional que ao abrir uma sessão de usuário ele deve atribuir as variáveis ORACLE\_HOME e LD\_LIBRARY_PATH, necessárias para o funcionamento do **Oracle Instant Client**.

Feche a sessão atual e faça login novamente para forçar o carregamento destes novos parâmetros.

Feito isto certifique-se que funcionou executando o comando abaixo e veja a saída, ela deve apontar para o caminho da instalação do **Oracle Instant Client**, conforme colocamos no arquivo **instantclient.sh**:

<div class="wp_codebox">
  <table>
    <tr id="p1447229">
      <td class="code" id="p1447code229">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #007800;">$ORACLE_HOME</span></pre>
      </td>
    </tr>
  </table>
</div>

A saída para o comando deve ser algo como isto abaixo se você tiver a mesma versão do **Oracle Instant Client** que eu usei.

<div class="wp_codebox">
  <table>
    <tr id="p1447230">
      <td class="code" id="p1447code230">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">11.2</span><span style="color: #000000; font-weight: bold;">/</span>client</pre>
      </td>
    </tr>
  </table>
</div>

Joinha, mais um pouco eu te liberto desta saga, pois agora ainda temos que ajustar algumas configurações.

Crie o arquivo _cx_Oracle.conf_, com o conteúdo a seguir:

<div class="wp_codebox">
  <table>
    <tr id="p1447231">
      <td class="code" id="p1447code231">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">11.2</span><span style="color: #000000; font-weight: bold;">/</span>client<span style="color: #000000; font-weight: bold;">/</span>lib</pre>
      </td>
    </tr>
  </table>
</div>

> Ta bom, vou falar novamente, mas não esqueça por favor: O diretório acima na sua instalação pode ser outro.

Copie este arquivo para _/etc/ld.so.conf.d/_:

<div class="wp_codebox">
  <table>
    <tr id="p1447232">
      <td class="code" id="p1447code232">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">cp</span> cx_Oracle.conf <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>ld.so.conf.d<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

Execute o comando abaixo para atualizar os paths das libs:

<div class="wp_codebox">
  <table>
    <tr id="p1447233">
      <td class="code" id="p1447code233">
        <pre class="bash" style="font-family:monospace;">ldconfig</pre>
      </td>
    </tr>
  </table>
</div>

<center>
  &#8220;Agora vem a parte mais fácil da história.&#8221;
</center>

Precisamos fazer o **Python 2.6** acessar **Oracle** a partir do **Oracle Instant Client**, para realizar esta &#8220;mágica&#8221;, basta instalar a extensão **cx_Oracle** no **Python 2.6**. 

Sempre é bom começar pelo básico, mas essencial, então faça download do cx_Oracle a partir do link abaixo:

<a href="http://prdownloads.sourceforge.net/cx-oracle/cx_Oracle-5.1.1.tar.gz?download" target="_blank">Clique para baixar cx_Oracle</a>

Para instalar a partir de um pacote tarball precisaremos compilar a extension cx_Oracle novamente, assim temos que ter certeza de que nosso ambiente pode fazer isto.

A sua vez é agora, tenha certeza de ter instalado o pacote de desenvolvimento do **Python 2.6** e o compilador **gcc**, faça isto com o comando que segue:

<div class="wp_codebox">
  <table>
    <tr id="p1447234">
      <td class="code" id="p1447code234">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> python26-devel <span style="color: #c20cb9; font-weight: bold;">gcc</span></pre>
      </td>
    </tr>
  </table>
</div>

Ok! Já sabemos que nosso ambiente será capaz de compilar a cx_Oracle e já fizemos o download, agora descompacte o arquivo do **cx_Oracle**:

<div class="wp_codebox">
  <table>
    <tr id="p1447235">
      <td class="code" id="p1447code235">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">tar</span> zxvf cx_Oracle-5.1.1.tar.gz</pre>
      </td>
    </tr>
  </table>
</div>

Entre na pasta que tu acabou de extrair:

<div class="wp_codebox">
  <table>
    <tr id="p1447236">
      <td class="code" id="p1447code236">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">cd</span> cx_Oracle-5.1.1</pre>
      </td>
    </tr>
  </table>
</div>

Agora agradeça o <a href="http://pt.wikipedia.org/wiki/Guido_van_Rossum" title="De uma olhada no Wikipédia e veja que é este cara!" target="_blank">Guido van Rossum</a> e depois execute o comando abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p1447237">
      <td class="code" id="p1447code237">
        <pre class="bash" style="font-family:monospace;">python26 setup.py <span style="color: #c20cb9; font-weight: bold;">install</span></pre>
      </td>
    </tr>
  </table>
</div>

Umas &#8220;coisas&#8221; como estas devem ter sido exibidas pra ti:

<div class="wp_codebox">
  <table>
    <tr id="p1447238">
      <td class="code" id="p1447code238">
        <pre class="bash" style="font-family:monospace;">running <span style="color: #c20cb9; font-weight: bold;">install</span>
running build
running build_ext
building <span style="color: #ff0000;">'cx_Oracle'</span> extension
<span style="color: #c20cb9; font-weight: bold;">gcc</span> <span style="color: #660033;">-pthread</span> <span style="color: #660033;">-fno-strict-aliasing</span> <span style="color: #660033;">-O2</span> <span style="color: #660033;">-g</span> <span style="color: #660033;">-pipe</span> <span style="color: #660033;">-Wall</span> -Wp,-D_FORTIFY_SOURCE=<span style="color: #000000;">2</span> <span style="color: #660033;">-fexceptions</span> <span style="color: #660033;">-fstack-protector</span> <span style="color: #660033;">--param</span>=ssp-buffer-size=<span style="color: #000000;">4</span> <span style="color: #660033;">-m32</span> <span style="color: #660033;">-march</span>=i386 <span style="color: #660033;">-mtune</span>=generic <span style="color: #660033;">-fasynchronous-unwind-tables</span> -D_GNU_SOURCE <span style="color: #660033;">-fPIC</span> <span style="color: #660033;">-fwrapv</span> -I<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>kerberos<span style="color: #000000; font-weight: bold;">/</span>include <span style="color: #660033;">-DNDEBUG</span> <span style="color: #660033;">-O2</span> <span style="color: #660033;">-g</span> <span style="color: #660033;">-pipe</span> <span style="color: #660033;">-Wall</span> -Wp,-D_FORTIFY_SOURCE=<span style="color: #000000;">2</span> <span style="color: #660033;">-fexceptions</span> <span style="color: #660033;">-fstack-protector</span> <span style="color: #660033;">--param</span>=ssp-buffer-size=<span style="color: #000000;">4</span> <span style="color: #660033;">-m32</span> <span style="color: #660033;">-march</span>=i386 <span style="color: #660033;">-mtune</span>=generic <span style="color: #660033;">-fasynchronous-unwind-tables</span> -D_GNU_SOURCE <span style="color: #660033;">-fPIC</span> <span style="color: #660033;">-fwrapv</span> <span style="color: #660033;">-fPIC</span> -I<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>include<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">11.2</span><span style="color: #000000; font-weight: bold;">/</span>client -I<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>include<span style="color: #000000; font-weight: bold;">/</span>python2.6 <span style="color: #660033;">-c</span> cx_Oracle.c <span style="color: #660033;">-o</span> build<span style="color: #000000; font-weight: bold;">/</span>temp.linux-i686-<span style="color: #000000;">2.6</span>-11g<span style="color: #000000; font-weight: bold;">/</span>cx_Oracle.o -DBUILD_VERSION=5.1.1
creating build<span style="color: #000000; font-weight: bold;">/</span>lib.linux-i686-<span style="color: #000000;">2.6</span>-11g
<span style="color: #c20cb9; font-weight: bold;">gcc</span> <span style="color: #660033;">-pthread</span> <span style="color: #660033;">-shared</span> build<span style="color: #000000; font-weight: bold;">/</span>temp.linux-i686-<span style="color: #000000;">2.6</span>-11g<span style="color: #000000; font-weight: bold;">/</span>cx_Oracle.o -L<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">11.2</span><span style="color: #000000; font-weight: bold;">/</span>client<span style="color: #000000; font-weight: bold;">/</span>lib -L<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib <span style="color: #660033;">-lclntsh</span> -lpython2.6 <span style="color: #660033;">-o</span> build<span style="color: #000000; font-weight: bold;">/</span>lib.linux-i686-<span style="color: #000000;">2.6</span>-11g<span style="color: #000000; font-weight: bold;">/</span>cx_Oracle.so
running install_lib
copying build<span style="color: #000000; font-weight: bold;">/</span>lib.linux-i686-<span style="color: #000000;">2.6</span>-11g<span style="color: #000000; font-weight: bold;">/</span>cx_Oracle.so -<span style="color: #000000; font-weight: bold;">&gt;</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>python2.6<span style="color: #000000; font-weight: bold;">/</span>site-packages
running install_egg_info
Writing <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>python2.6<span style="color: #000000; font-weight: bold;">/</span>site-packages<span style="color: #000000; font-weight: bold;">/</span>cx_Oracle-5.1.1-py2.6.egg-info</pre>
      </td>
    </tr>
  </table>
</div>

Caraca, agora já deve estar funcionado, vamos testar?

Execute os comandos abaixo, simples assim:

<div class="wp_codebox">
  <table>
    <tr id="p1447239">
      <td class="code" id="p1447code239">
        <pre class="bash" style="font-family:monospace;">python26</pre>
      </td>
    </tr>
  </table>
</div>

Bem vindo ao **Python 2.6**:

<div class="wp_codebox">
  <table>
    <tr id="p1447240">
      <td class="code" id="p1447code240">
        <pre class="bash" style="font-family:monospace;">Python 2.6.8 <span style="color: #7a0874; font-weight: bold;">&#40;</span>unknown, Apr <span style="color: #000000;">12</span> <span style="color: #000000;">2012</span>, <span style="color: #000000;">20</span>:<span style="color: #000000;">59</span>:00<span style="color: #7a0874; font-weight: bold;">&#41;</span> 
<span style="color: #7a0874; font-weight: bold;">&#91;</span>GCC 4.1.2 <span style="color: #000000;">20080704</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Red Hat 4.1.2-<span style="color: #000000;">52</span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> on linux2
Type <span style="color: #ff0000;">"help"</span>, <span style="color: #ff0000;">"copyright"</span>, <span style="color: #ff0000;">"credits"</span> or <span style="color: #ff0000;">"license"</span> <span style="color: #000000; font-weight: bold;">for</span> <span style="color: #c20cb9; font-weight: bold;">more</span> information.
<span style="color: #000000; font-weight: bold;">&gt;&gt;&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Tente importar o módulo, se não retornar nenhum erro <del datetime="2012-06-15T18:00:27+00:00">sorria você está sendo filmado</del> parabéns.

<div class="wp_codebox">
  <table>
    <tr id="p1447241">
      <td class="code" id="p1447code241">
        <pre class="python" style="font-family:monospace;"><span style="color: #66cc66;">&gt;&gt;&gt;</span> <span style="color: #ff7700;font-weight:bold;">import</span> cx_Oracle
<span style="color: #66cc66;">&gt;&gt;&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Meu serviço termina aqui, o seu deve estar só começando.

Lembrando, agora vou configurar este servidor para ser um proxy reverso Nginx, quando terminar faço referência aqui.