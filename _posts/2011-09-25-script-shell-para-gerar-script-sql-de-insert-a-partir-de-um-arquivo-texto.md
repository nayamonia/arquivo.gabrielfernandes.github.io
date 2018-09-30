---
id: 800
title: Script shell para gerar script SQL de INSERT a partir de um arquivo texto
date: 2011-09-25T19:12:21+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=800
permalink: /2011/09/25/script-shell-para-gerar-script-sql-de-insert-a-partir-de-um-arquivo-texto/
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
categories:
  - banco de dados
  - linux
  - shell
  - sql
tags:
  - script
  - sql
---
O **shell** do Linux, sem dúvida é um ótimo ambiente para se trabalhar com fluxos de texto, pois dispõe de uma ampla variedade de programas para manipular arquivos de texto.

Ele pode ajudar quando temos que lidar com grandes volumes de informações em arquivos texto. Um exemplo pode ser a criação de scripts **SQL** para inserir uma grande quantidade de informações em uma base de dados a partir de um **arquivo texto** que contenha estes dados.

Facilmente podemos criar um **script shell** capaz de ler o conteúdo de um arquivo texto e criar scripts **SQL** de **INSERT** destas informações para inserir em um banco de dados.
  
<!--more [CONTINUAR LENDO]-->

Para mostrar como fazer vamos criar uma oportunidade: 

**A hipótese**

  * Há um arquivo texto chamado _CLIENTES.TXT_ que contém o código e nome do cliente, no qual precisamos inserir os clientes deste arquivo em uma tabela do banco de dados chamada _TAB_CLIENTE_. 
  * A _TAB_CLIENTE_ tem apenas três campos, sendo eles: _COD_CLIENTE_, _DES_CLIENTE_ e _VAL\_LIMITE\_CREDITO_. 
  * O campo do limite deve ser igual a &#8220;45.00&#8221;, independente do cliente.</blockquote> 
  * Exemplo de conteúdo do arquivo _CLIENTES.TXT_: 
<div class="wp_codebox">
  <table>
    <tr id="p800131">
      <td class="code" id="p800code131">
        <pre class="csv" style="font-family:monospace;">9999337665277;JOSE DA SILVA
9999549280275;JOAO SILVEIRA
9999745441708;MARIA DA LUZ
9999200854463;BENTA GERTRUDES
9999659208107;PEDRO DE SOUZA
9999948839890;BENTO GONCALVES
9999847556034;LEANDRO DE MATOS</pre>
      </td>
    </tr>
  </table>
</div>

**O que o script shell precisa fazer?**

Basicamente é necessário carregar cada linha do arquivo em uma variável, extrair as informações do código e nome do cliente, montar o **script SQL** e criar um arquivo com os **scripts SQL** gerados.

**Como fazer?**

Defina o script para inserir um cliente na base de dados manualmente, exemplo:</p> 

<div class="wp_codebox">
  <table>
    <tr id="p800132">
      <td class="code" id="p800code132">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999337665277</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'JOSE DA SILVA'</span> <span style="color: #66cc66;">&#41;</span>;</pre>
      </td>
    </tr>
  </table>
</div>

Analise o **script SQL** e observe que queremos gerar um arquivo texto contendo um **script SQL** como este por linha.
  
Para cada uma das linhas devemos substituir o código e o nome do cliente por aqueles do arquivo texto de clientes. 

A construção do **script shell** deve iniciar pela criação de variáveis na qual armazenaremos trechos do **script SQL** para que possamos inserir o código e nome do cliente na posição correta. 

Serão definidas três variáveis:

  * **sql0**: Armazena o conteúdo do **script SQL** até o local onde devemos inserir o código do cliente.
<div class="wp_codebox">
  <table>
    <tr id="p800133">
      <td class="code" id="p800code133">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span></pre>
      </td>
    </tr>
  </table>
</div>

  * **sql1**: Armazena o conteúdo entre o código e nome do cliente, ou seja, apenas a vírgula e a aspa.
<div class="wp_codebox">
  <table>
    <tr id="p800134">
      <td class="code" id="p800code134">
        <pre class="sql" style="font-family:monospace;"><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'</span></pre>
      </td>
    </tr>
  </table>
</div>

  * **sql2**: Armazena o final do **script SQL**.
<div class="wp_codebox">
  <table>
    <tr id="p800135">
      <td class="code" id="p800code135">
        <pre class="sql" style="font-family:monospace;"><span style="color: #ff0000;">' );</span></pre>
      </td>
    </tr>
  </table>
</div>

Atribua estes valores nas variáveis **sql0**, **sql1** e **sql2** no **shell script**:

<div class="wp_codebox">
  <table>
    <tr id="p800136">
      <td class="code" id="p800code136">
        <pre class="bash" style="font-family:monospace;"><span style="color: #007800;">sql0</span>=<span style="color: #ff0000;">"INSERT INTO tab_cliente (val_limite_credito, cod_cliente, des_cliente) VALUES (45.00, "</span>
<span style="color: #007800;">sql1</span>=<span style="color: #ff0000;">", '"</span>
<span style="color: #007800;">sql2</span>=<span style="color: #ff0000;">"' );"</span></pre>
      </td>
    </tr>
  </table>
</div>

Para ler linha a linha do arquivo texto de clientes podemos usar o comando _while_.
  
O exemplo abaixo armazena na variável **linha** cada uma das linhas do arquivo _CLIENTES.TXT_ e exibe na tela o conteúdo lido através do comando _echo_:

<div class="wp_codebox">
  <table>
    <tr id="p800137">
      <td class="code" id="p800code137">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">read</span> linha; <span style="color: #000000; font-weight: bold;">do</span>
   <span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"<span style="color: #007800;">$linha</span>"</span>;
<span style="color: #000000; font-weight: bold;">done</span> <span style="color: #000000; font-weight: bold;">&lt;</span> CLIENTES.TXT</pre>
      </td>
    </tr>
  </table>
</div>

Vamos separar da variável _linha_ o campo código do cliente do campo nome do cliente com a expansão de variáveis no shell afim de capturar trechos da variável linha:

  * No exemplo que segue a variável _coluna1_ recebe o conteúdo da variável _linha_ a partir da posição _N_ até a quantidade de caracteres de _tamanho_.
<div class="wp_codebox">
  <table>
    <tr id="p800138">
      <td class="code" id="p800code138">
        <pre class="bash" style="font-family:monospace;"><span style="color: #007800;">coluna1</span>=<span style="color: #800000;">${linha:N:tamanho}</span></pre>
      </td>
    </tr>
  </table>
</div>

Para entender melhor podemos modificar o script _while_ fazendo com que seja exibido apenas o nome do cliente na tela, capturando os 50 caracteres a partir da coluna 14. 

Observe o código abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p800139">
      <td class="code" id="p800code139">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">read</span> linha; <span style="color: #000000; font-weight: bold;">do</span>
   <span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"<span style="color: #007800;">${linha:14:50}</span>"</span>;
<span style="color: #000000; font-weight: bold;">done</span> <span style="color: #000000; font-weight: bold;">&lt;</span> CLIENTES.TXT</pre>
      </td>
    </tr>
  </table>
</div>

Modificaremos o trecho do _while_ de tal forma que possamos em conjunto das variáveis **sql0**, **sql1** e **sql2** montar o **script SQL** e exibir na tela:

<div class="wp_codebox">
  <table>
    <tr id="p800140">
      <td class="code" id="p800code140">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">read</span> linha; <span style="color: #000000; font-weight: bold;">do</span>
   <span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"<span style="color: #007800;">$sql0</span><span style="color: #007800;">${linha:0:13}</span><span style="color: #007800;">$sql1</span><span style="color: #007800;">${linha:14:50}</span><span style="color: #007800;">$sql2</span>"</span>
<span style="color: #000000; font-weight: bold;">done</span> <span style="color: #000000; font-weight: bold;">&lt;</span> CLIENTES.TXT</pre>
      </td>
    </tr>
  </table>
</div>

Para salvar em um arquivo texto, redirecione a saída padrão para um arquivo, observe o exemplo para inserir o resultado no arquivo _INSERT_CLIENTES.SQL_:

<div class="wp_codebox">
  <table>
    <tr id="p800141">
      <td class="code" id="p800code141">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">read</span> linha; <span style="color: #000000; font-weight: bold;">do</span>
   <span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"<span style="color: #007800;">$sql0</span><span style="color: #007800;">${linha:0:13}</span><span style="color: #007800;">$sql1</span><span style="color: #007800;">${linha:14:50}</span><span style="color: #007800;">$sql2</span>"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> INSERT_CLIENTES.SQL;
<span style="color: #000000; font-weight: bold;">done</span> <span style="color: #000000; font-weight: bold;">&lt;</span> CLIENTES.TXT</pre>
      </td>
    </tr>
  </table>
</div>

**Resultado final:**

Ao juntar as partes, devemos ter um script shell assim:</p> 

<div class="wp_codebox">
  <table>
    <tr id="p800142">
      <td class="code" id="p800code142">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;">#!/bin/bash</span>
<span style="color: #666666; font-style: italic;"># @autor Gabriel Fernandes </span>
&nbsp;
<span style="color: #007800;">sql0</span>=<span style="color: #ff0000;">"INSERT INTO tab_cliente (val_limite_credito, cod_cliente, des_cliente) VALUES (45.00, "</span>
<span style="color: #007800;">sql1</span>=<span style="color: #ff0000;">", '"</span>
<span style="color: #007800;">sql2</span>=<span style="color: #ff0000;">"' );"</span>
&nbsp;
<span style="color: #000000; font-weight: bold;">while</span> <span style="color: #c20cb9; font-weight: bold;">read</span> linha; <span style="color: #000000; font-weight: bold;">do</span>
   <span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"<span style="color: #007800;">$sql0</span><span style="color: #007800;">${linha:0:13}</span><span style="color: #007800;">$sql1</span><span style="color: #007800;">${linha:14:50}</span><span style="color: #007800;">$sql2</span>"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> INSERT_CLIENTES.SQL;
<span style="color: #000000; font-weight: bold;">done</span> <span style="color: #000000; font-weight: bold;">&lt;</span> CLIENTES.TXT</pre>
      </td>
    </tr>
  </table>
</div>

Exemplo de conteúdo do arquivo _INSERT_CLIENTES.SQL_ criado:

<div class="wp_codebox">
  <table>
    <tr id="p800143">
      <td class="code" id="p800code143">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999337665277</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'JOSE DA SILVA'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999549280275</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'JOAO SILVEIRA'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999745441708</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'MARIA DA LUZ'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999200854463</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'BENTA GERTRUDES'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999659208107</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'PEDRO DE SOUZA'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999948839890</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'BENTO GONCALVES'</span> <span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> tab_cliente <span style="color: #66cc66;">&#40;</span>val_limite_credito<span style="color: #66cc66;">,</span> cod_cliente<span style="color: #66cc66;">,</span> des_cliente<span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">45.00</span><span style="color: #66cc66;">,</span> <span style="color: #cc66cc;">9999847556034</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'LEANDRO DE MATOS'</span> <span style="color: #66cc66;">&#41;</span>;</pre>
      </td>
    </tr>
  </table>
</div>