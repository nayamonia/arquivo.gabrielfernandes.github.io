---
id: 2013
title: Erro ao abrir Oracle SQL Developer 4 no Fedora
date: 2014-12-29T14:00:16+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=2013
permalink: /2014/12/29/erro-ao-abrir-oracle-sql-developer-4-no-fedora/
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
image: /wp-content/uploads/2014/12/Captura-de-tela-de-2014-12-29-151004.png
categories:
  - banco de dados
  - linux
tags:
  - fedora
  - oracle
  - sqldeveloper
---
Seguiu os passos do post **[Instalar Oracle SQL Developer no Fedora](http://wp.me/p1KyJn-df)** ou **[Instalar Oracle SQL Developer 4 no Fedora](http://wp.me/p1KyJn-wi)** e tá estourando o erro abaixo?

<!--more [CONTINUAR LENDO]-->

<div class="wp_codebox">
  <table>
    <tr id="p2013283">
      <td class="code" id="p2013code283">
        <pre class="bash" style="font-family:monospace;">&nbsp;
 Oracle SQL Developer
 Copyright <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000;">1997</span>, <span style="color: #000000;">2014</span>, Oracle and<span style="color: #000000; font-weight: bold;">/</span>or its affiliates. All rights reserved.
&nbsp;
 LOAD TIME : <span style="color: #000000;">549</span><span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># A fatal error has been detected by the Java Runtime Environment:</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;">#  SIGSEGV (0xb) at pc=0x00007fd81d373910, pid=9388, tid=140566764693248</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># JRE version: Java(TM) SE Runtime Environment (7.0_71-b14) (build 1.7.0_71-b14)</span>
<span style="color: #666666; font-style: italic;"># Java VM: Java HotSpot(TM) 64-Bit Server VM (24.71-b01 mixed mode linux-amd64 compressed oops)</span>
<span style="color: #666666; font-style: italic;"># Problematic frame:</span>
<span style="color: #666666; font-style: italic;"># C  0x00007fd81d373910</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># Core dump written. Default location: /opt/sqldeveloper/sqldeveloper/bin/core or core.9388</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># An error report file with more information is saved as:</span>
<span style="color: #666666; font-style: italic;"># /tmp/hs_err_pid9388.log</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #666666; font-style: italic;"># If you would like to submit a bug report, please visit:</span>
<span style="color: #666666; font-style: italic;">#   http://bugreport.sun.com/bugreport/crash.jsp</span>
<span style="color: #666666; font-style: italic;">#</span>
<span style="color: #000000; font-weight: bold;">/</span>opt<span style="color: #000000; font-weight: bold;">/</span>sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>bin<span style="color: #000000; font-weight: bold;">/</span>..<span style="color: #000000; font-weight: bold;">/</span>..<span style="color: #000000; font-weight: bold;">/</span>ide<span style="color: #000000; font-weight: bold;">/</span>bin<span style="color: #000000; font-weight: bold;">/</span>launcher.sh: line <span style="color: #000000;">1193</span>:  <span style="color: #000000;">9388</span> Abortado                <span style="color: #7a0874; font-weight: bold;">&#40;</span>imagem <span style="color: #000000; font-weight: bold;">do</span> núcleo gravada<span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #800000;">${JAVA}</span> <span style="color: #ff0000;">"<span style="color: #007800;">${APP_VM_OPTS[@]}</span>"</span> <span style="color: #800000;">${APP_ENV_VARS}</span> <span style="color: #660033;">-classpath</span> <span style="color: #800000;">${APP_CLASSPATH}</span> <span style="color: #800000;">${APP_MAIN_CLASS}</span> <span style="color: #ff0000;">"<span style="color: #007800;">${APP_APP_OPTS[@]}</span>"</span></pre>
      </td>
    </tr>
  </table>
</div>

Calma, calma! A solução é fácil, achei neste link http://blog.allanglesit.com/2014/07/sql-developer-crash-on-fedora-20/ &#8230; vou transcrever:

Aparentemente o **Oracle SQL Developer** prevê a variável **GNOME\_DESKTOP\_SESSION_ID** que não é mais utilizada nas versões recentes do GNOME. Pra resolver, basta dar um **unset** nela antes de executar o **Oracle SQL Developer**. 

Eu fiz assim, igualzinho o post do link acima, segue:

1- Editei o arquivo /opt/sqldeveloper/sqldeveloper.sh usando o root:

<div class="wp_codebox">
  <table>
    <tr id="p2013284">
      <td class="code" id="p2013code284">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>opt<span style="color: #000000; font-weight: bold;">/</span>sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>sqldeveloper.sh</pre>
      </td>
    </tr>
  </table>
</div>

2- Depois adicionei a linha abaixo, antes da chamada do programa sqldeveloper:

<div class="wp_codebox">
  <table>
    <tr id="p2013285">
      <td class="code" id="p2013code285">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">unset</span> <span style="color: #660033;">-v</span> GNOME_DESKTOP_SESSION_ID</pre>
      </td>
    </tr>
  </table>
</div>

3- Meu arquivo /opt/sqldeveloper/sqldeveloper.sh final ficou assim:

<div class="wp_codebox">
  <table>
    <tr id="p2013286">
      <td class="code" id="p2013code286">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;">#!/bin/bash</span>
<span style="color: #7a0874; font-weight: bold;">unset</span> <span style="color: #660033;">-v</span> GNOME_DESKTOP_SESSION_ID
<span style="color: #7a0874; font-weight: bold;">cd</span> <span style="color: #ff0000;">"<span style="color: #780078;">`dirname $0`</span>"</span><span style="color: #000000; font-weight: bold;">/</span>sqldeveloper<span style="color: #000000; font-weight: bold;">/</span>bin <span style="color: #000000; font-weight: bold;">&&</span> <span style="color: #c20cb9; font-weight: bold;">bash</span> sqldeveloper <span style="color: #007800;">$*</span></pre>
      </td>
    </tr>
  </table>
</div>

Valeu.