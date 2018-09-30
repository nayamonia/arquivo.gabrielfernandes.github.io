---
id: 1578
title: 'Como alterar local padrão do &#8220;Documents/Aptana Rubles&#8221; no Aptana Studio 3'
date: 2012-10-30T09:19:26+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1578
permalink: /2012/10/30/alterando-local-da-ruble-user-aptana-studio-3/
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
image: /wp-content/uploads/2012/10/studio3.png
categories:
  - desenvolvimento
  - linux
  - programação
tags:
  - aptana
---
Se tu, assim como eu, acha um _pé na orelha_ o fato do **Aptana Studio 3** criar a pasta **~/Documents/Aptana Rubles** no home do usuário toda vez que ele inicia.

Pode ficar tranqüilo querido! Tu acabou de encontrar a solução e acredite é muito ridícula de tão simples.
  
<!--more [CONTINUAR LENDO]-->

Edite o arquivo de configuração do **Aptana Studio 3**, o _AptanaStudio3.ini_ que encontra-se na pasta onde o **Aptana Studio 3** foi instalado. O conteúdo antes da alteração deve ser parecido com este abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p1578261">
      <td class="code" id="p1578code261">
        <pre class="java" style="font-family:monospace;"><span style="color: #339933;">-</span>startup
plugins<span style="color: #339933;">/</span>org.<span style="color: #006633;">eclipse</span>.<span style="color: #006633;">equinox</span>.<span style="color: #006633;">launcher_1</span>.2.0.<span style="color: #006633;">v20110502</span>.<span style="color: #006633;">jar</span>
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">library</span>
plugins<span style="color: #339933;">/</span>org.<span style="color: #006633;">eclipse</span>.<span style="color: #006633;">equinox</span>.<span style="color: #006633;">launcher</span>.<span style="color: #006633;">gtk</span>.<span style="color: #006633;">linux</span>.<span style="color: #006633;">x86_64_1</span>.1.100.<span style="color: #006633;">v20110505</span>
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">XXMaxPermSize</span>
256m
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">defaultAction</span>
openFile
<span style="color: #339933;">-</span>vmargs
<span style="color: #339933;">-</span>Xms40m
<span style="color: #339933;">-</span>Xmx512m
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">p2</span>.<span style="color: #006633;">unsignedPolicy</span><span style="color: #339933;">=</span>allow
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">log</span>.<span style="color: #006633;">size</span>.<span style="color: #006633;">max</span><span style="color: #339933;">=</span><span style="color: #cc66cc;">10000</span>
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">log</span>.<span style="color: #006633;">backup</span>.<span style="color: #006633;">max</span><span style="color: #339933;">=</span><span style="color: #cc66cc;">5</span>
<span style="color: #339933;">-</span>Djava.<span style="color: #006633;">awt</span>.<span style="color: #006633;">headless</span><span style="color: #339933;">=</span><span style="color: #000066; font-weight: bold;">true</span></pre>
      </td>
    </tr>
  </table>
</div>

Agora inclua no final do arquivo a linha _-Dstudio.rubleUserLocation=/home/gabriel/Workspaces/_, lembre-se de substituir o local **/home/gabriel/Workspaces/**, pelo caminho da tua workspace.

O arquivo depois de alterado, deve ficar mais ou menos assim:

<div class="wp_codebox">
  <table>
    <tr id="p1578262">
      <td class="code" id="p1578code262">
        <pre class="java" style="font-family:monospace;"><span style="color: #339933;">-</span>startup
plugins<span style="color: #339933;">/</span>org.<span style="color: #006633;">eclipse</span>.<span style="color: #006633;">equinox</span>.<span style="color: #006633;">launcher_1</span>.2.0.<span style="color: #006633;">v20110502</span>.<span style="color: #006633;">jar</span>
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">library</span>
plugins<span style="color: #339933;">/</span>org.<span style="color: #006633;">eclipse</span>.<span style="color: #006633;">equinox</span>.<span style="color: #006633;">launcher</span>.<span style="color: #006633;">gtk</span>.<span style="color: #006633;">linux</span>.<span style="color: #006633;">x86_64_1</span>.1.100.<span style="color: #006633;">v20110505</span>
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">XXMaxPermSize</span>
256m
<span style="color: #339933;">--</span>launcher.<span style="color: #006633;">defaultAction</span>
openFile
<span style="color: #339933;">-</span>vmargs
<span style="color: #339933;">-</span>Xms40m
<span style="color: #339933;">-</span>Xmx512m
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">p2</span>.<span style="color: #006633;">unsignedPolicy</span><span style="color: #339933;">=</span>allow
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">log</span>.<span style="color: #006633;">size</span>.<span style="color: #006633;">max</span><span style="color: #339933;">=</span><span style="color: #cc66cc;">10000</span>
<span style="color: #339933;">-</span>Declipse.<span style="color: #006633;">log</span>.<span style="color: #006633;">backup</span>.<span style="color: #006633;">max</span><span style="color: #339933;">=</span><span style="color: #cc66cc;">5</span>
<span style="color: #339933;">-</span>Djava.<span style="color: #006633;">awt</span>.<span style="color: #006633;">headless</span><span style="color: #339933;">=</span><span style="color: #000066; font-weight: bold;">true</span>
<span style="color: #339933;">-</span>Dstudio.<span style="color: #006633;">rubleUserLocation</span><span style="color: #339933;">=/</span>home<span style="color: #339933;">/</span>gabriel<span style="color: #339933;">/</span>Workspaces<span style="color: #339933;">/</span></pre>
      </td>
    </tr>
  </table>
</div>

Boa Sorte!