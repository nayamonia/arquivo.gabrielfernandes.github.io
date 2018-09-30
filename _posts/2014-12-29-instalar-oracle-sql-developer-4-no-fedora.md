---
id: 2002
title: Instalar Oracle SQL Developer 4 no Fedora
date: 2014-12-29T12:56:00+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=2002
permalink: /2014/12/29/instalar-oracle-sql-developer-4-no-fedora/
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
image: /wp-content/uploads/2014/12/oraclesqldeveloper4.png
categories:
  - banco de dados
  - linux
tags:
  - fedora
  - oracle
  - sqldeveloper
---
O **Oracle SQL Developer 4** é uma ferramenta gratuita que simplifica muito as tarefas de desenvolvimento de banco de dados. Nela podemos procurar objetos de banco de dados, executar instruções **SQL** e scripts **SQL**, editar e depurar instruções PL / **SQL**. Também é possível criar e salvar relatórios personalizados. 

No **Fedora** 20 podemos instalar o **Oracle SQL Developer 4** da seguinte maneira:

<!--more [CONTINUAR LENDO]-->

> Tá precisando instalar uma versão mais antiga do **Oracle SQL Developer no Fedora**, entre neste outro post **[Instalar Oracle SQL Developer no Fedora](http://wp.me/p1KyJn-df "Clique e não perca mais tempo pesquisando, você já achou o que precisava.")** e não perca mais tempo pesquisando!

> Acabou de instalar e está com erro ao abrir? Veja como resolver neste meu outro post **[Erro ao abrir Oracle SQL Developer 4 no Fedora](http://wp.me/p1KyJn-wt "Clique e confira a solução agora!")**

Baixe o pacote RPM da instalação do **Oracle SQL Developer 4**, será necessário efetuar cadastro gratuito na Oracle para efetuar o download.

<a href="http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html" target="_blank">Link para a área de download do <strong>Oracle SQL Developer</strong></a>

Neste site, procure pela versão RPM para Linux (**Linux RPM**), encontre o link para o **Oracle SQL Developer RPM Linux**.

O arquivo que eu baixei, foi o _sqldeveloper-4.0.3.16.84-1.noarch.rpm_, versão mais recente no dia em que escrevi isto, mas quando você for baixar a versão pode ser diferente, fique ligado!

Para instalar usei o _yum_, assim:</del>:

<div class="wp_codebox">
  <table>
    <tr id="p2002277">
      <td class="code" id="p2002code277">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> sqldeveloper-4.0.3.16.84-<span style="color: #000000;">1</span>.noarch.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Agora baixe o Java SE Development Kit 7, importante lembrar que o **Oracle SQL Developer 4** funciona apenas com a versão 7 ou superior.

<a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Link para a área de download do Java SE Development Kit 7</a>

> Você também pode baixar SDK do mesmo site em que baixou o **Oracle SQL Developer 4**, se você voltar lá agora, clicando no <a href="http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html" target="_blank">Link para a área de download do <strong>Oracle SQL Developer</strong></a> vai ver que tem um link lá que aponta para **<a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" title="Clique aqui e vá direto para a área de download do SDK 7" target="_blank">SQL Developer requires JDK 7 or above</a>**. A escolha é sua! 🙂
> 
> [<img src="https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/12/Captura-de-tela-de-2014-12-29-132521.png?resize=300%2C124" alt="JDK 7" width="300" height="124" class="aligncenter size-medium wp-image-2004" srcset="https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/12/Captura-de-tela-de-2014-12-29-132521.png?resize=300%2C124 300w, https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/12/Captura-de-tela-de-2014-12-29-132521.png?w=557 557w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/12/Captura-de-tela-de-2014-12-29-132521.png) 

> Tentei instalar com o Java SE Development Kit 8 e ao abrir o **Oracle SQL Developer 4** ele reclamou da versão alegando que poderia não funcionar corretamente, então deixei a Java SE Development Kit 7 mesmo. 

No site de download clique no link para baixar o JDK 7 que você será redirecionado para a área de download do JDK 7, procure pela versão **Linux x86** ou **Linux x64**, dependendo da sua arquitetura e faça o download da versão em RPM.

Eu instalei o pacote assim:

<div class="wp_codebox">
  <table>
    <tr id="p2002278">
      <td class="code" id="p2002code278">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> jdk-7u71-linux-x64.rpm</pre>
      </td>
    </tr>
  </table>
</div>

Agora temos que informar ao **Oracle SQL Developer 4** o caminho onde foi instalado o _JDK 7_.
  
Para realizar esta tarefa devemos saber em qual pasta ele foi instalado, por padrão a pasta do _JDK_ está abaixo da pasta _/usr/java/_, entre nesta pasta e verifique o caminho correto, você deve ter um conteúdo como este:

<div class="wp_codebox">
  <table>
    <tr id="p2002279">
      <td class="code" id="p2002code279">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ls</span> <span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>java<span style="color: #000000; font-weight: bold;">/</span>
default  jdk1.7.0_71  latest</pre>
      </td>
    </tr>
  </table>
</div>

Para nosso exemplo o caminho é _/usr/java/jdk1.7.0_71/_, portanto vamos salvar esta informação no arquivo _~/.sqldeveloper/4.0.0/product.conf_ para que o **Oracle SQL Developer 4** saiba onde está instalado o _JDK_. Opa isto mesmo, na versão **Oracle SQL Developer 4** o arquivo que guarda esta configuração mudou de ~/.sqldeveloper/jdk para _**~/.sqldeveloper/4.0.0/product.conf**_

Use o comando abaixo, alterando para a sua realidade:

<div class="wp_codebox">
  <table>
    <tr id="p2002280">
      <td class="code" id="p2002code280">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">mkdir</span> <span style="color: #660033;">-p</span> ~<span style="color: #000000; font-weight: bold;">/</span>.sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>4.0.0
<span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"SetJavaHome /usr/java/jdk1.7.0_71/"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> ~<span style="color: #000000; font-weight: bold;">/</span>.sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>4.0.0<span style="color: #000000; font-weight: bold;">/</span>product.conf</pre>
      </td>
    </tr>
  </table>
</div>

Legal, agora já podemos iniciar o _sqldeveloper_ pela primeira vez.

Na linha de comando use:

<div class="wp_codebox">
  <table>
    <tr id="p2002281">
      <td class="code" id="p2002code281">
        <pre class="bash" style="font-family:monospace;">sqldeveloper</pre>
      </td>
    </tr>
  </table>
</div>

Se por acaso uma mensagem como esta abaixo for exibida, significa que o caminho para o JDK não está correto, revise o caminho colocado no arquivo _~/.sqldeveloper/4.0.0/product.conf_.

<div class="wp_codebox">
  <table>
    <tr id="p2002282">
      <td class="code" id="p2002code282">
        <pre class="bash" style="font-family:monospace;">&nbsp;
 Oracle SQL Developer
 Copyright <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000;">1997</span>, <span style="color: #000000;">2014</span>, Oracle and<span style="color: #000000; font-weight: bold;">/</span>or its affiliates. All rights reserved.
&nbsp;
Type the full pathname of a JDK installation <span style="color: #7a0874; font-weight: bold;">&#40;</span>or Ctrl-C to quit<span style="color: #7a0874; font-weight: bold;">&#41;</span>, the path will be stored <span style="color: #000000; font-weight: bold;">in</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>.sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>4.0.0<span style="color: #000000; font-weight: bold;">/</span>product.conf</pre>
      </td>
    </tr>
  </table>
</div>