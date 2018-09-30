---
id: 110
title: SQL para exportar tabela com campo DATE no Oracle
date: 2011-06-27T18:44:53+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=110
permalink: /2011/06/27/sql-para-exportar-tabela-com-campo-date-no-oracle/
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
  - sql
tags:
  - exportar
  - oracle
  - sql
  - tabela
---
Trabalhar com datas em banco de dados é popularmente uma tarefa meio chatinho e cheia de poréns.

Portanto quero compartilhar um caso onde tive que exportar uma tabela de um banco de dados Oracle e importar em outro.
  
A situação foi especial porque a única ferramenta disponível era a linha de comando SQL e como havia campos DATE, o formato da saída para um SELECT não era o mesmo aceito na hora de inserir no outro banco de dados Oracle, gerando erro na operação. Eu também não podia usar o export porque no outro banco já haviam dados e não podia perde-los.

<!--more-->

Após algumas pesquisas encontrei uma solução relativamente simples para o caso, vamos à ela:
  
Para entender melhor sugiro criar esta tabela abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p11014">
      <td class="code" id="p110code14">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">CREATE</span> <span style="color: #993333; font-weight: bold;">TABLE</span> usuario <span style="color: #66cc66;">&#40;</span>
    <span style="color: #ff0000;">"codigo"</span> NUMBER <span style="color: #993333; font-weight: bold;">NOT</span> <span style="color: #993333; font-weight: bold;">NULL</span> ENABLE<span style="color: #66cc66;">,</span> 
    <span style="color: #ff0000;">"nome"</span> VARCHAR2<span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">100</span> BYTE<span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">,</span> 
    <span style="color: #ff0000;">"ultimologin"</span> DATE <span style="color: #66cc66;">,</span> 
    CONSTRAINT <span style="color: #ff0000;">"pk_usuario"</span> <span style="color: #993333; font-weight: bold;">PRIMARY</span> <span style="color: #993333; font-weight: bold;">KEY</span> <span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"codigo"</span><span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">&#41;</span>;</pre>
      </td>
    </tr>
  </table>
</div>

Insira estes dois registros na tabela de exemplo:

<div class="wp_codebox">
  <table>
    <tr id="p11015">
      <td class="code" id="p110code15">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> usuario <span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"codigo"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"nome"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"ultimologin"</span><span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">1</span><span style="color: #66cc66;">,</span><span style="color: #ff0000;">'gabriel'</span><span style="color: #66cc66;">,</span> TO_DATE<span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">'22/09/1829 07:21'</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'DD/MM/RR HH24:mi'</span><span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">&#41;</span>;
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> usuario <span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"codigo"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"nome"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"ultimologin"</span><span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">2</span><span style="color: #66cc66;">,</span><span style="color: #ff0000;">'tu'</span><span style="color: #66cc66;">,</span> TO_DATE<span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">'21/07/1929 17:35'</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'DD/MM/RR HH24:mi'</span><span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">&#41;</span>;</pre>
      </td>
    </tr>
  </table>
</div>

Pronto, temos o ambiente para testar.

Agora vamos gerar um SQL Script de SELECT que irá gerar na saída um SQL Script de INSERT, note que isto não é segredo nenhum, a novidade é o uso do TO_CHAR para deixar a data no formato correto para inserção na outra tabela sem problemas. Veja como fica o Script para o exemplo dado:

<div class="wp_codebox">
  <table>
    <tr id="p11016">
      <td class="code" id="p110code16">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">SELECT</span> <span style="color: #ff0000;">'INSERT INTO usuario ("codigo", "nome", "ultimologin") VALUES ('</span> <span style="color: #66cc66;">||</span> 
       <span style="color: #ff0000;">"codigo"</span> <span style="color: #66cc66;">||</span> <span style="color: #ff0000;">','</span><span style="color: #ff0000;">''</span> <span style="color: #66cc66;">||</span> 
       <span style="color: #ff0000;">"nome"</span>   <span style="color: #66cc66;">||</span> <span style="color: #ff0000;">''</span><span style="color: #ff0000;">''</span>  <span style="color: #66cc66;">||</span> 
       <span style="color: #ff0000;">',TO_DATE('</span><span style="color: #ff0000;">''</span> <span style="color: #66cc66;">||</span> TO_CHAR<span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"ultimologin"</span><span style="color: #66cc66;">,</span><span style="color: #ff0000;">'DD/MM/YYYY HH24:MI'</span><span style="color: #66cc66;">&#41;</span> <span style="color: #66cc66;">||</span><span style="color: #ff0000;">''</span><span style="color: #ff0000;">', '</span><span style="color: #ff0000;">'DD/MM/RR HH24:mi'</span><span style="color: #ff0000;">'));'</span>
<span style="color: #993333; font-weight: bold;">FROM</span> usuario</pre>
      </td>
    </tr>
  </table>
</div>

A saída deste Script deve ser como esta a seguir, ou seja, um Script de INSERT prontinho para ser executado no outro Banco de Dados:

<div class="wp_codebox">
  <table>
    <tr id="p11017">
      <td class="code" id="p110code17">
        <pre class="sql" style="font-family:monospace;"><span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> usuario <span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"codigo"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"nome"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"ultimologin"</span><span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">1</span><span style="color: #66cc66;">,</span><span style="color: #ff0000;">'gabriel'</span><span style="color: #66cc66;">,</span>TO_DATE<span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">'22/09/1829 07:21'</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'DD/MM/RR HH24:mi'</span><span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">&#41;</span>;                                                                                                                                     
<span style="color: #993333; font-weight: bold;">INSERT</span> <span style="color: #993333; font-weight: bold;">INTO</span> usuario <span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">"codigo"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"nome"</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">"ultimologin"</span><span style="color: #66cc66;">&#41;</span> <span style="color: #993333; font-weight: bold;">VALUES</span> <span style="color: #66cc66;">&#40;</span><span style="color: #cc66cc;">2</span><span style="color: #66cc66;">,</span><span style="color: #ff0000;">'tu'</span><span style="color: #66cc66;">,</span>TO_DATE<span style="color: #66cc66;">&#40;</span><span style="color: #ff0000;">'21/07/1929 17:35'</span><span style="color: #66cc66;">,</span> <span style="color: #ff0000;">'DD/MM/RR HH24:mi'</span><span style="color: #66cc66;">&#41;</span><span style="color: #66cc66;">&#41;</span>;</pre>
      </td>
    </tr>
  </table>
</div>

É isso ai.