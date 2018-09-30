---
id: 821
title: Instalar Oracle SQL Developer no Fedora
date: 2011-09-15T19:11:48+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=821
permalink: /2011/09/15/instalar-oracle-sql-developer-no-fedora/
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
image: /wp-content/uploads/2011/09/sqldev151.png
categories:
  - banco de dados
  - linux
tags:
  - fedora
  - oracle
  - sqldeveloper
---
O **Oracle SQL Developer** é uma ferramenta gratuita que simplifica muito as tarefas de desenvolvimento de banco de dados. Nela podemos procurar objetos de banco de dados, executar instruções **SQL** e scripts **SQL**, editar e depurar instruções PL / **SQL**. Também é possível criar e salvar relatórios personalizados. 

No **Fedora** 15 podemos instalar o **Oracle SQL Developer** da seguinte maneira:

<!--more [CONTINUAR LENDO]-->

> Estas procurando como instalar a mais nova versão, ou seja, o **Oracle SQL Developer 4 no Fedora**, entre neste outro post **[Instalar Oracle SQL Developer 4 no Fedora](http://wp.me/p1KyJn-wi "Clique e não perca mais tempo pesquisando, você já achou o que precisava.")** e não perca mais tempo pesquisando!

Baixe o pacote RPM da instalação do **Oracle SQL Developer**, é necessário um cadastro gratuito, ao tentar baixar será solicitado um usuário, se não tiver um, você deve inscrever-se.

<a href="http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html" target="_blank">Link para a área de download do <strong>Oracle SQL Developer</strong></a>

Neste site, procure pela versão RPM para Linux, encontre o link para o **Oracle SQL Developer RPM for Linux**.

O arquivo que eu baixei, foi o _sqldeveloper-3.0.04.34-1.noarch.rpm_, mas quando você for baixar a versão pode ser diferente, fique atento!

Para instalar use o _yum_, pois caso falte alguma dependência ela será resolvida automaticamente <del datetime="2011-09-15T22:06:18+00:00">há exceções!</del>:

<div class="wp_codebox">
  <table>
    <tr id="p821124">
      <td class="code" id="p821code124">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> sqldeveloper-3.0.04.34-<span style="color: #000000;">1</span>.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Agora baixe o Java SE Development Kit 6, importante lembrar que atualmente o **Oracle SQL Developer** funciona apenas com a versão 6.

<a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Link para a área de download do Java SE Development Kit 6</a>

No site de download clique no link para baixar o JDK 6 que você será redirecionado para a área de download do JDK 6, procure pela versão **Linux x86 &#8211; RPM Installer** e faça o download.

O pacote RPM do JDK é &#8220;auto instalável&#8221;, siga os passos a seguir para instalar:

<div class="wp_codebox">
  <table>
    <tr id="p821125">
      <td class="code" id="p821code125">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chmod</span> +x jdk-6u27-linux-i586-rpm.bin</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p821126">
      <td class="code" id="p821code126">
        <pre class="bash" style="font-family:monospace;">.<span style="color: #000000; font-weight: bold;">/</span>jdk-6u27-linux-i586-rpm.bin</pre>
      </td>
    </tr>
  </table>
</div>

Informe ao **Oracle SQL Developer** o caminho onde foi instalado o _JDK 6_.
  
Para realizar esta tarefa devemos saber em qual pasta ele foi instalado, por padrão a pasta do _JDK_ está abaixo da pasta _/usr/java/_, entre nesta pasta e verifique o caminho correto, você deve ter um conteúdo como este:

<div class="wp_codebox">
  <table>
    <tr id="p821127">
      <td class="code" id="p821code127">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ls</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>java<span style="color: #000000; font-weight: bold;">/</span>
default  jdk1.6.0_27  latest</pre>
      </td>
    </tr>
  </table>
</div>

Para nosso exemplo o caminho é _/usr/java/jdk1.6.0_27/_, portanto vamos salvar esta informação no arquivo _~/.sqldeveloper/jdk_ para que o **Oracle SQL Developer** saiba onde está instalado o _JDK_. 

Use o comando abaixo, alterando para a sua realidade:

<div class="wp_codebox">
  <table>
    <tr id="p821128">
      <td class="code" id="p821code128">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>java<span style="color: #000000; font-weight: bold;">/</span>jdk1.6.0_27<span style="color: #000000; font-weight: bold;">/</span> <span style="color: #000000; font-weight: bold;">&gt;</span> ~<span style="color: #000000; font-weight: bold;">/</span>.sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>jdk</pre>
      </td>
    </tr>
  </table>
</div>

Legal, agora já podemos iniciar o _sqldeveloper_ pela primeira vez.

Na linha de comando use:

<div class="wp_codebox">
  <table>
    <tr id="p821129">
      <td class="code" id="p821code129">
        <pre class="bash" style="font-family:monospace;">sqldeveloper</pre>
      </td>
    </tr>
  </table>
</div>

Se por acaso uma mensagem como esta abaixo for exibida, significa que o caminho para o JDK não está correto, revise o caminho colocado no arquivo _~/.sqldeveloper/jdk_.

<div class="wp_codebox">
  <table>
    <tr id="p821130">
      <td class="code" id="p821code130">
        <pre class="bash" style="font-family:monospace;">Oracle SQL Developer
 Copyright <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000;">1997</span>, <span style="color: #000000;">2011</span>, Oracle and<span style="color: #000000; font-weight: bold;">/</span>or its affiliates. All rights reserved. 
&nbsp;
Type the full pathname of a J2SE installation <span style="color: #7a0874; font-weight: bold;">&#40;</span>or Ctrl-C to quit<span style="color: #7a0874; font-weight: bold;">&#41;</span>, the path will be stored <span style="color: #000000; font-weight: bold;">in</span> ~<span style="color: #000000; font-weight: bold;">/</span>.sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>jdk</pre>
      </td>
    </tr>
  </table>
</div>