---
id: 300
title: 'Como configurar o PHP com suporte Oracle no Fedora &#8211; extension oci8'
date: 2011-08-01T15:29:57+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=300
permalink: /2011/08/01/como-fazer-php-funcionar-com-oracle-oci8-no-fedora-15/
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
  - banco de dados
  - driver
  - linux
  - shell
tags:
  - extension
  - fedora
  - oci8
  - oracle
  - php
---
Continuando a montagem do meu ambiente **Fedora 15**, surge a necessidade de fazer o **PHP** conectar no **Oracle**.

No meu caso, não existe a necessidade do banco de dados completo instalado em minha máquina, pois só preciso conectar-me a uma ou mais instancias **Oracle** de em um servidor remoto e já tenho o PHP funcionando normalmente. 

Para tornar esta tarefa relativamente simples no **Fedora**, vamos utilizar o pacote **Instant Client** da **Oracle**. 

Vejamos como isto pode ser feito:

<!--more [CONTINUAR LENDO]-->

Vá no site da **Oracle** e faça o download dos pacotes **RPMs** do **Instant Client Basic** e **SDK**.
  
**ATENÇÃO**: Para fazer download na Oracle é necessário efetuar um cadastro simples e gratuito.

[**Instant Client Package** &#8211; _Basic: All files required to run OCI, OCCI, and JDBC-OCI applications_](http://download.oracle.com/otn/linux/instantclient/112020/oracle-instantclient11.2-basic-11.2.0.2.0.i386.rpm)</p> 

[**Instant Client Package** &#8211; _SDK: Additional header files and an example makefile for developing_](http://download.oracle.com/otn/linux/instantclient/112020/oracle-instantclient11.2-devel-11.2.0.2.0.i386.rpm) </ul> 

Os links acima tentam efetuar download das mesmas versões que utilizei, caso tenha dificuldades com eles, ou deseja baixar outras versões ou outra arquitetura, como x86_64, você deve acessar um dos links abaixo no qual levará você para a página principal e do download do Oracle Instant Client, consequentemente:

<a href="http://www.oracle.com/technetwork/database/features/instant-client/index.html" target="_blank">Página do Oracle Database Instant Client</a>

<a href="http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html" target="_blank">Página das opções de Download do Oracle Database Instant Client</a>

Lembre-se de duas coisas na hora de baixar: 

. Para baixar você precisa cadastrar-se.
  
. Baixe os pacotes RPMs, com nome igual aos que baixei, apenas com número de versão diferente.

De posso dos pacotes **Basic** e **SDK** do **Oracle Instant Client**, podemos prosseguir instalando os pacotes baixados, para isto, podemos usar o comando **rpm** como a seguir. Lembrando que se você baixou uma versão diferente, o nome do arquivo poderá ser diferente.

<div class="wp_codebox">
  <table>
    <tr id="p30057">
      <td class="code" id="p300code57">
        <pre class="bash" style="font-family:monospace;">rpm <span style="color: #660033;">-ivh</span> oracle-instantclient11.2-basic-11.2.0.2.0.i386.rpm
rpm <span style="color: #660033;">-ivh</span> oracle-instantclient11.2-devel-11.2.0.2.0.i386.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Agora que já devemos ter os pacotes devidamente instalados, podemos instalar a extensão **OCI8** no **PHP**, siga as instruções a seguir.

Carregar na variável **LD\_LIBRARY\_PATH** o caminho padrão do **Instant Client**:

<div class="wp_codebox">
  <table>
    <tr id="p30058">
      <td class="code" id="p300code58">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">export</span> <span style="color: #007800;">LD_LIBRARY_PATH</span>=<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>oracle<span style="color: #000000; font-weight: bold;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

Agora podemos instalar a **OCI8** usando o **<a href="http://www.php.net/manual/en/install.pecl.php" target="_blank">PECL</a>**, um repositório de extensões **PHP** disponível pelo sistema de pacote <a href="http://pear.php.net/" target="_blank"><strong>PEAR</strong></a>. Digite o comando como segue:

<div class="wp_codebox">
  <table>
    <tr id="p30059">
      <td class="code" id="p300code59">
        <pre class="bash" style="font-family:monospace;">pecl <span style="color: #c20cb9; font-weight: bold;">install</span> oci8</pre>
      </td>
    </tr>
  </table>
</div>

Quando for solicitado o caminho para ORACLE_HOME, simplesmente pressione [ENTER] para que seja localizado automaticamente. A mensagem que solicita isto deve ser como esta do exemplo abaixo:

> Please provide the path to the ORACLE_HOME directory. Use &#8216;instantclient,/path/to/instant/client/lib&#8217; if you&#8217;re compiling with Oracle Instant Client [autodetect] : 

Terminado a instalação, devemos informar ao **PHP** que a extensão **OCI8** está disponível e pode ser carregada. No Fedora podemos fazer isto criando o arquivo **oci8.ini** na pasta das configurações adicionais do **PHP**, pasta /etc/php.d/.

Use o **vi** para criar e editar o arquivo **/etc/php.d/oci8.ini**:

<div class="wp_codebox">
  <table>
    <tr id="p30060">
      <td class="code" id="p300code60">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>php.d<span style="color: #000000; font-weight: bold;">/</span>oci8.ini</pre>
      </td>
    </tr>
  </table>
</div>

Insira o conteúdo abaixo no arquivo, a primeira linha é apenas um comentário, já a segunda é indispensável para carregar a extensão **OCI8** no **PHP**:

<div class="wp_codebox">
  <table>
    <tr id="p30061">
      <td class="code" id="p300code61">
        <pre class="bash" style="font-family:monospace;">; Enable oci8 extension module
<span style="color: #007800;">extension</span>=oci8.so</pre>
      </td>
    </tr>
  </table>
</div>

Se tudo correu bem, seu **PHP** está configurado para conectar-se ao banco de dados **Oracle**.