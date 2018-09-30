---
id: 1359
title: Configurar o CentOS 5 para suporte ao Tornado no Python 2.6
date: 2012-06-15T12:48:53+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1359
permalink: /2012/06/15/configurar-o-centos-5-para-suporte-ao-tornado-no-python-26/
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
image: /wp-content/uploads/2012/06/o-fotografo-cacador-de-tempestades-brad-mack-tira-fotos-do-tornado-em-kansas-nos-eua-1334490984120_956x500.jpg
categories:
  - banco de dados
  - desenvolvimento
  - linux
  - shell
tags:
  - centos
  - python
  - tornado
---
A instalação do **CentOS** utilizada para escrever este post é resultado dos procedimentos publicados no post <a href="http://www.gabrielfernandes.org/2012/06/04/como-criar-e-instalar-o-centos-em-uma-maquina-virtual-no-servidor-xen-centos-5/" title="Clique e veja como criar uma VM e instalar o CentOS em um Servidor Xen" target="_blank">Como criar uma máquina virtual no servidor <strong>Xen Centos 5</strong> e instalar o <strong>CentOS</strong> pela rede</a> aqui mesmo na [Compostagem Digital](http://cd2.com.br "Se clicar aqui, você voltará para a página principal do blog"), portanto para ter uma instalação como esta de uma conferida nele.

A versão do **Tornado** da minha aplicação é a 2.1.1, que possui como pré-requisito o **Python** 2.6, item não disponível no **CentOS 5.8** (Final), portanto o primeiro passo é instalar a versão **Python** requerida pela sua aplicação, eu fiz assim:
  
<!--more [CONTINUAR LENDO]-->

Adicionei o repositório <a href="http://fedoraproject.org/wiki/EPEL" title="Clicando aqui, uma nova janela será aberta e você ira para o site do repositorio EPEL" target="_blank">EPEL</a> à minha instalação

> Em resumo, o Extra Packages for Enterprise Linux (ou EPEL) é um grupo de usuários Fedora com interesse especial que cria, mantém e gerencia um conjunto de pacotes adicionais de alta qualidade para &#8220;Enterprise Linux&#8221;, que pode ser usado no Red Hat Enterprise Linux (RHEL) e CentOS. 

Adicionei o repositório EPEL à minha instalação **CentOS** com o seguinte comando:

<div class="wp_codebox">
  <table>
    <tr id="p1359202">
      <td class="code" id="p1359code202">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-ivh</span> http:<span style="color: #000000; font-weight: bold;">//</span>epel.gtdinternet.com<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>epel-release-<span style="color: #000000;">5</span>-<span style="color: #000000;">4</span>.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

> Opa!!! O comando acima falhou? Pode ser porque o pacote do EPEL tenha sido atualizado, caso tenha problemas, verifique ou baixe o arquivo no link <a href="http://epel.gtdinternet.com/5/i386/repoview/epel-release.html" title="Vai lá dar uma olha no que pode estar errado." target="_blank">http://epel.gtdinternet.com/5/i386/repoview/epel-release.html</a> 

Se não falhou, algo como o conteúdo abaixo deve ter sido exibido no seu terminal:

<div class="wp_codebox">
  <table>
    <tr id="p1359203">
      <td class="code" id="p1359code203">
        <pre class="bash" style="font-family:monospace;">Obtendo http:<span style="color: #000000; font-weight: bold;">//</span>epel.gtdinternet.com<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>epel-release-<span style="color: #000000;">5</span>-<span style="color: #000000;">4</span>.noarch.rpm
aviso: <span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>tmp<span style="color: #000000; font-weight: bold;">/</span>rpm-xfer.JEimUb: Cabeçalho V3 assinatura DSA: NOKEY, key ID 217521f6
Preparando...               <span style="color: #666666; font-style: italic;">########################################### [100%]</span>
   <span style="color: #000000;">1</span>:epel-release           <span style="color: #666666; font-style: italic;">########################################### [100%]</span></pre>
      </td>
    </tr>
  </table>
</div>

Ok, tu já deves estar ansioso para instalar o **Python** 2.6 ou talvez não, mas eu já estou&#8230;

Segue o comando:

<div class="wp_codebox">
  <table>
    <tr id="p1359204">
      <td class="code" id="p1359code204">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> python26</pre>
      </td>
    </tr>
  </table>
</div>

Se tudo correr bem, você deve estar visualizando algo como a tela a seguir:

<div class="wp_codebox">
  <table>
    <tr id="p1359205">
      <td class="code" id="p1359code205">
        <pre class="bash" style="font-family:monospace;">Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
 <span style="color: #000000; font-weight: bold;">*</span> base: centos.pop.com.br
 <span style="color: #000000; font-weight: bold;">*</span> epel: mirror.cogentco.com
 <span style="color: #000000; font-weight: bold;">*</span> extras: centos.pop.com.br
 <span style="color: #000000; font-weight: bold;">*</span> updates: centos.pop.com.br
Setting up Install Process
Resolving Dependencies
--<span style="color: #000000; font-weight: bold;">&gt;</span> Running transaction check
---<span style="color: #000000; font-weight: bold;">&gt;</span> Package python26.i386 <span style="color: #000000;"></span>:2.6.8-<span style="color: #000000;">1</span>.el5 <span style="color: #000000; font-weight: bold;">set</span> to be updated
--<span style="color: #000000; font-weight: bold;">&gt;</span> Processing Dependency: libpython2.6.so.1.0 <span style="color: #000000; font-weight: bold;">for</span> package: python26
--<span style="color: #000000; font-weight: bold;">&gt;</span> Processing Dependency: libffi.so.5 <span style="color: #000000; font-weight: bold;">for</span> package: python26
--<span style="color: #000000; font-weight: bold;">&gt;</span> Running transaction check
---<span style="color: #000000; font-weight: bold;">&gt;</span> Package libffi.i386 <span style="color: #000000;"></span>:3.0.5-<span style="color: #000000;">1</span>.el5 <span style="color: #000000; font-weight: bold;">set</span> to be updated
---<span style="color: #000000; font-weight: bold;">&gt;</span> Package python26-libs.i386 <span style="color: #000000;"></span>:2.6.8-<span style="color: #000000;">1</span>.el5 <span style="color: #000000; font-weight: bold;">set</span> to be updated
--<span style="color: #000000; font-weight: bold;">&gt;</span> Finished Dependency Resolution
&nbsp;
Dependencies Resolved
&nbsp;
============================================================================================================
 Package                       Arch                 Version                      Repository            Size
============================================================================================================
Installing:
 python26                      i386                 2.6.8-<span style="color: #000000;">1</span>.el5                  epel                 <span style="color: #000000;">6.5</span> M
Installing <span style="color: #000000; font-weight: bold;">for</span> dependencies:
 libffi                        i386                 3.0.5-<span style="color: #000000;">1</span>.el5                  epel                  <span style="color: #000000;">21</span> k
 python26-libs                 i386                 2.6.8-<span style="color: #000000;">1</span>.el5                  epel                 <span style="color: #000000;">670</span> k
&nbsp;
Transaction Summary
============================================================================================================
Install       <span style="color: #000000;">3</span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Upgrade       <span style="color: #000000;"></span> Package<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span>
&nbsp;
Total download <span style="color: #c20cb9; font-weight: bold;">size</span>: <span style="color: #000000;">7.2</span> M
Is this ok <span style="color: #7a0874; font-weight: bold;">&#91;</span>y<span style="color: #000000; font-weight: bold;">/</span>N<span style="color: #7a0874; font-weight: bold;">&#93;</span>: y</pre>
      </td>
    </tr>
  </table>
</div>

Experimente responder &#8220;N&#8221; na pergunta acima pra tu ver onde vais chegar &#8230; lol !!! A lugar nenhum é claro e sinceramente, se respondeu &#8220;N&#8221; tu precisas confiar mais naquilo que faz &#8230; 🙂

Brincadeiras a parte, responda &#8220;y&#8221; para prosseguir com a instalação e uma &#8220;coisa&#8221; como esta abaixo deve ser exibida:

<div class="wp_codebox">
  <table>
    <tr id="p1359206">
      <td class="code" id="p1359code206">
        <pre class="bash" style="font-family:monospace;">Downloading Packages:
<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: libffi-3.0.5-<span style="color: #000000;">1</span>.el5.i386.rpm                                                   <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000;">21</span> kB     00:00     
<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">2</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: python26-libs-2.6.8-<span style="color: #000000;">1</span>.el5.i386.rpm                                            <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">670</span> kB     00:01     
<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">3</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: python26-2.6.8-<span style="color: #000000;">1</span>.el5.i386.rpm                                                 <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">6.5</span> MB     00:<span style="color: #000000;">31</span>     
<span style="color: #660033;">------------------------------------------------------------------------------------------------------------</span>
Total                                                                       <span style="color: #000000;">212</span> kB<span style="color: #000000; font-weight: bold;">/</span>s <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">7.2</span> MB     00:<span style="color: #000000;">34</span>     
aviso: rpmts_HdrFromFdno: Cabeçalho V4 assinatura DSA: NOKEY, key ID 217521f6
epel<span style="color: #000000; font-weight: bold;">/</span>gpgkey                                                                          <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000;">1.7</span> kB     00:00     
Importing GPG key 0x217521F6 <span style="color: #ff0000;">"Fedora EPEL &lt;epel@fedoraproject.org&gt;"</span> from <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>pki<span style="color: #000000; font-weight: bold;">/</span>rpm-gpg<span style="color: #000000; font-weight: bold;">/</span>RPM-GPG-KEY-EPEL
Is this ok <span style="color: #7a0874; font-weight: bold;">&#91;</span>y<span style="color: #000000; font-weight: bold;">/</span>N<span style="color: #7a0874; font-weight: bold;">&#93;</span>: y</pre>
      </td>
    </tr>
  </table>
</div>

Quantas perguntas, isto é Linux, sempre pergunta ao seu mestre (root) se pode ou não fazer algo.
  
Aqui o Yum está avisando que o &#8220;hash&#8221; de identificação dos pacotes para &#8220;Fedora EPEL&#8221; não está &#8220;instalado&#8221;, responda &#8220;y&#8221; para importar a GPG Key para concluir a instalação.

<div class="wp_codebox">
  <table>
    <tr id="p1359207">
      <td class="code" id="p1359code207">
        <pre class="bash" style="font-family:monospace;">Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : libffi                                                                               <span style="color: #000000;">1</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span> 
  Installing     : python26                                                                             <span style="color: #000000;">2</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span> 
  Installing     : python26-libs                                                                        <span style="color: #000000;">3</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">3</span> 
&nbsp;
Installed:
  python26.i386 <span style="color: #000000;"></span>:2.6.8-<span style="color: #000000;">1</span>.el5                                                                               
&nbsp;
Dependency Installed:
  libffi.i386 <span style="color: #000000;"></span>:3.0.5-<span style="color: #000000;">1</span>.el5                         python26-libs.i386 <span style="color: #000000;"></span>:2.6.8-<span style="color: #000000;">1</span>.el5                        
&nbsp;
Complete<span style="color: #000000; font-weight: bold;">!</span></pre>
      </td>
    </tr>
  </table>
</div>

Voilá, &#8220;Complete!&#8221; é uma boa resposta. Coisa linda!!!

Faça um teste do **Python** 2.6, simples assim, execute:

<div class="wp_codebox">
  <table>
    <tr id="p1359208">
      <td class="code" id="p1359code208">
        <pre class="bash" style="font-family:monospace;">python26</pre>
      </td>
    </tr>
  </table>
</div>

Confira a versão do **Python**:

<div class="wp_codebox">
  <table>
    <tr id="p1359209">
      <td class="code" id="p1359code209">
        <pre class="bash" style="font-family:monospace;">Python 2.6.8 <span style="color: #7a0874; font-weight: bold;">&#40;</span>unknown, Apr <span style="color: #000000;">12</span> <span style="color: #000000;">2012</span>, <span style="color: #000000;">20</span>:<span style="color: #000000;">59</span>:00<span style="color: #7a0874; font-weight: bold;">&#41;</span> 
<span style="color: #7a0874; font-weight: bold;">&#91;</span>GCC 4.1.2 <span style="color: #000000;">20080704</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Red Hat 4.1.2-<span style="color: #000000;">52</span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> on linux2
Type <span style="color: #ff0000;">"help"</span>, <span style="color: #ff0000;">"copyright"</span>, <span style="color: #ff0000;">"credits"</span> or <span style="color: #ff0000;">"license"</span> <span style="color: #000000; font-weight: bold;">for</span> <span style="color: #c20cb9; font-weight: bold;">more</span> information.
<span style="color: #000000; font-weight: bold;">&gt;&gt;&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

E depois faça qualquer &#8220;coisa&#8221;, só pra ter mais certeza ainda que está tudo bem:

<div class="wp_codebox">
  <table>
    <tr id="p1359210">
      <td class="code" id="p1359code210">
        <pre class="python" style="font-family:monospace;"><span style="color: #66cc66;">&gt;&gt;&gt;</span> <span style="color: #ff7700;font-weight:bold;">print</span> <span style="color: #483d8b;">"Quando escrevi este artigo estava olhando para o mar da janela do QG"</span>
Quando escrevi este artigo estava olhando para o mar da janela do QG
<span style="color: #66cc66;">&gt;&gt;&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Que bom, tudo parece estar bem.

Agora é a vez do **Tornado**, este é bem &#8220;facinho&#8221; de fazer. 

<center>
  &#8220;<em>Esse <strong>Python</strong> faz coisa!</em>&#8220;
</center>

Comece baixando a versão Tornado que tu precisas, faça uma visitinha no site do link abaixo e volte pra cá, estou te esperando:

<a href="https://github.com/facebook/tornado/downloads" title="Clique, acesse a área de download do Tornado e volte pra cá." target="_blank">https://github.com/facebook/tornado/downloads</a>

Eu baixei a 2.1.1 pois como dito anteriormente é a versão pré-requisito para minha aplicação, se for baixar a mesma pode usar os comandos que seguem:

<div class="wp_codebox">
  <table>
    <tr id="p1359211">
      <td class="code" id="p1359code211">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">cd</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>local<span style="color: #000000; font-weight: bold;">/</span>src<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p1359212">
      <td class="code" id="p1359code212">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">wget</span> https:<span style="color: #000000; font-weight: bold;">//</span>github.com<span style="color: #000000; font-weight: bold;">/</span>downloads<span style="color: #000000; font-weight: bold;">/</span>facebook<span style="color: #000000; font-weight: bold;">/</span>tornado<span style="color: #000000; font-weight: bold;">/</span>tornado-2.1.1.tar.gz</pre>
      </td>
    </tr>
  </table>
</div>

> É possível que tu estejas sem entender o porque do &#8220;cd /usr/local/src/&#8221;, a resposta é simples: É uma questão de organização, tu podes fazer sob o diretório que desejar.

Enquanto o download do **Tornado** ocorre, você deve estar visualizando algo como as linhas a seguir:

<div class="wp_codebox">
  <table>
    <tr id="p1359213">
      <td class="code" id="p1359code213">
        <pre class="bash" style="font-family:monospace;"><span style="color: #660033;">--2012-06-05</span> <span style="color: #000000;">10</span>:<span style="color: #000000;">47</span>:<span style="color: #000000;">23</span>--  https:<span style="color: #000000; font-weight: bold;">//</span>github.com<span style="color: #000000; font-weight: bold;">/</span>downloads<span style="color: #000000; font-weight: bold;">/</span>facebook<span style="color: #000000; font-weight: bold;">/</span>tornado<span style="color: #000000; font-weight: bold;">/</span>tornado-2.1.1.tar.gz
Resolvendo github.com... 207.97.227.239
A conectar github.com<span style="color: #000000; font-weight: bold;">|</span>207.97.227.239<span style="color: #000000; font-weight: bold;">|</span>:<span style="color: #000000;">443</span>... conectado<span style="color: #000000; font-weight: bold;">!</span>
HTTP requisição enviada, aguardando resposta... <span style="color: #000000;">302</span> Found
Localização: http:<span style="color: #000000; font-weight: bold;">//</span>cloud.github.com<span style="color: #000000; font-weight: bold;">/</span>downloads<span style="color: #000000; font-weight: bold;">/</span>facebook<span style="color: #000000; font-weight: bold;">/</span>tornado<span style="color: #000000; font-weight: bold;">/</span>tornado-2.1.1.tar.gz <span style="color: #7a0874; font-weight: bold;">&#91;</span>seguinte<span style="color: #7a0874; font-weight: bold;">&#93;</span>
<span style="color: #660033;">--2012-06-05</span> <span style="color: #000000;">10</span>:<span style="color: #000000;">47</span>:<span style="color: #000000;">24</span>--  http:<span style="color: #000000; font-weight: bold;">//</span>cloud.github.com<span style="color: #000000; font-weight: bold;">/</span>downloads<span style="color: #000000; font-weight: bold;">/</span>facebook<span style="color: #000000; font-weight: bold;">/</span>tornado<span style="color: #000000; font-weight: bold;">/</span>tornado-2.1.1.tar.gz
Resolvendo cloud.github.com... 205.251.223.23, 205.251.223.194, 205.251.223.154, ...
A conectar cloud.github.com<span style="color: #000000; font-weight: bold;">|</span>205.251.223.23<span style="color: #000000; font-weight: bold;">|</span>:<span style="color: #000000;">80</span>... conectado<span style="color: #000000; font-weight: bold;">!</span>
HTTP requisição enviada, aguardando resposta... <span style="color: #000000;">200</span> OK
Tamanho: <span style="color: #000000;">318796</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>311K<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span>application<span style="color: #000000; font-weight: bold;">/</span><span style="color: #c20cb9; font-weight: bold;">gzip</span><span style="color: #7a0874; font-weight: bold;">&#93;</span>
A gravar em: <span style="color: #ff0000;">'tornado-2.1.1.tar.gz'</span>
&nbsp;
<span style="color: #000000;">100</span><span style="color: #000000; font-weight: bold;">%</span><span style="color: #7a0874; font-weight: bold;">&#91;</span>==================================================================<span style="color: #000000; font-weight: bold;">&gt;</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> <span style="color: #000000;">318.796</span>      304K<span style="color: #000000; font-weight: bold;">/</span>s   em <span style="color: #000000;">1</span>,0s    
&nbsp;
<span style="color: #000000;">2012</span>-06-05 <span style="color: #000000;">10</span>:<span style="color: #000000;">47</span>:<span style="color: #000000;">25</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">304</span> KB<span style="color: #000000; font-weight: bold;">/</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span> - <span style="color: #ff0000;">'tornado-2.1.1.tar.gz'</span> gravado <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">318796</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">318796</span><span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

Terminado o download, descompacte o arquivo tarball do **Tornado**:

<div class="wp_codebox">
  <table>
    <tr id="p1359214">
      <td class="code" id="p1359code214">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">tar</span> zxvf tornado-2.1.1.tar.gz</pre>
      </td>
    </tr>
  </table>
</div>

Entre na pasta que tu acabou de descompactar:

<div class="wp_codebox">
  <table>
    <tr id="p1359215">
      <td class="code" id="p1359code215">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">cd</span> tornado-2.1.1</pre>
      </td>
    </tr>
  </table>
</div>

E pronto, faça a instalação com o comando a seguir:

<div class="wp_codebox">
  <table>
    <tr id="p1359216">
      <td class="code" id="p1359code216">
        <pre class="bash" style="font-family:monospace;">python26 setup.py <span style="color: #c20cb9; font-weight: bold;">install</span></pre>
      </td>
    </tr>
  </table>
</div>

Faça um teste bobão, tipo este.

Execute o Python 2.6:

<div class="wp_codebox">
  <table>
    <tr id="p1359217">
      <td class="code" id="p1359code217">
        <pre class="bash" style="font-family:monospace;">python26</pre>
      </td>
    </tr>
  </table>
</div>

E depois tente fazer um _import_ no **Tornado**:

<div class="wp_codebox">
  <table>
    <tr id="p1359218">
      <td class="code" id="p1359code218">
        <pre class="python" style="font-family:monospace;"><span style="color: #66cc66;">&gt;&gt;&gt;</span> <span style="color: #ff7700;font-weight:bold;">import</span> tornado
<span style="color: #66cc66;">&gt;&gt;&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Se nenhum erro foi mostrado no comando acima, sorria, pois está tudo pronto.

> Lembrando que para executar suas aplicações **Tornado** é necessário usar a versão 2.6 do **Python** invés da 2.4 e para não quebrar nenhum outro programa da instalação padrão do **CentOS** resolvendo o problema ajustando links simbólicos, sugiro incluir/alterar o cabeçalho em todos os arquivos .py da sua aplicação **Tornado**, como segue:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p1359219">
>       <td class="code" id="p1359code219">
>         <pre class="python" style="font-family:monospace;"><span style="color: #808080; font-style: italic;">#!/usr/bin/env python26</span></pre>
>       </td>
>     </tr>
>   </table>
> </div>

Após esta instalação, adicionei suporte ao **Oracle** no **Python 2.6**, coloquei os procedimentos desta alteração no post [Tornado e cx_Oracle no Python 2.6 em CentOS 5 com Oracle Instant Client](http://www.gabrielfernandes.org/2012/06/16/tornado-e-cxoracle-no-python-26-em-centos-5-com-oracle-instant-client/ "Clique e veja como fiz para subir o oracle instant client e o cx_Oracle")