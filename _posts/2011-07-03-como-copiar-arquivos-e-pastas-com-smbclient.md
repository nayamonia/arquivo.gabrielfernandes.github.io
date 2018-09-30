---
id: 129
title: Copiar arquivos e pastas entre Windows e Linux com Samba
date: 2011-07-03T18:22:22+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=129
permalink: /2011/07/03/como-copiar-arquivos-e-pastas-com-smbclient/
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
  - linux
  - rede
  - shell
tags:
  - linux
  - samba
  - windows
---
Copiar arquivos do Windows para o Linux e vice-versa é definitivamente uma tarefa bem comum, mas por vezes pode ser traumática e, até mesmo o &#8220;fim da picada&#8221; para alguns usuários de Linux.

Para a felicidade de todos, isto é apenas mito, pois é muito simples trocar arquivos pela rede entre estes dois sistemas operacionais, para tanto precisamos ter instalado no sistema Linux o programa **smbclient**.

Segue alguns exemplos práticos:

Para copiar um arquivo do Linux para um computador com o Windows (IP 192.168.12.201) no qual possui um compartilhamento com nome **teste**, podemos fazer assim:

<!--more-->

<div class="wp_codebox">
  <table>
    <tr id="p12918">
      <td class="code" id="p129code18">
        <pre class="bash" style="font-family:monospace;">smbclient <span style="color: #000000; font-weight: bold;">//</span>192.168.12.201<span style="color: #000000; font-weight: bold;">/</span>teste <span style="color: #660033;">-U</span> usuario <span style="color: #660033;">--pass</span> senha <span style="color: #660033;">-c</span> <span style="color: #ff0000;">"put /etc/hosts hosts;"</span></pre>
      </td>
    </tr>
  </table>
</div>

Este comando, vai copiar o arquivo /etc/hosts para //192.168.12.201/teste/, note que no exemplo você deve substituir **usuario** e **senha**, pelo nome do usuário e senha no Windows.

Agora fazendo o inverso, copiando do Windows para uma pasta no Linux:

<div class="wp_codebox">
  <table>
    <tr id="p12919">
      <td class="code" id="p129code19">
        <pre class="bash" style="font-family:monospace;">smbclient <span style="color: #000000; font-weight: bold;">//</span>192.168.12.201<span style="color: #000000; font-weight: bold;">/</span>teste <span style="color: #660033;">-U</span> guest <span style="color: #660033;">--pass</span> <span style="color: #ff0000;">""</span> <span style="color: #660033;">-c</span> <span style="color: #ff0000;">"get hosts /etc;"</span></pre>
      </td>
    </tr>
  </table>
</div>

Note que no comando acima, omitimos a senha, isto porque estamos considerando que o compartilhamento no Windows permite conexões de convidado sem senha.

Pode haver a necessidade de copiar pastas e subpastas completas, para tal podemos usar os comandos recurse, prompt e mget ou mput, dependendo se você desejar copiar de ou para um compartilhamento Windows. Siga os exemplos abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p12920">
      <td class="code" id="p129code20">
        <pre class="bash" style="font-family:monospace;">smbclient <span style="color: #000000; font-weight: bold;">//</span>192.168.12.201<span style="color: #000000; font-weight: bold;">/</span>teste <span style="color: #660033;">-U</span> guest <span style="color: #660033;">--pass</span> <span style="color: #ff0000;">""</span> <span style="color: #660033;">-c</span> <span style="color: #ff0000;">"recurse; prompt; mget Descanso*;"</span></pre>
      </td>
    </tr>
  </table>
</div>

O comando acima, copia todas as pastas e subpastas que estiverem dentro da pasta com nome **Descanso** do compartilhamento Windows para a pasta atual no Linux.

O contrário, seria assim:

<div class="wp_codebox">
  <table>
    <tr id="p12921">
      <td class="code" id="p129code21">
        <pre class="bash" style="font-family:monospace;">smbclient <span style="color: #000000; font-weight: bold;">//</span>192.168.12.201<span style="color: #000000; font-weight: bold;">/</span>teste <span style="color: #660033;">-U</span> guest <span style="color: #660033;">--pass</span> <span style="color: #ff0000;">""</span> <span style="color: #660033;">-c</span> <span style="color: #ff0000;">"recurse; prompt; mput Descanso*;"</span></pre>
      </td>
    </tr>
  </table>
</div>

Agora o comando acima está copiando a pasta **Descanso** para o compartilhamento Windows.

Por fim, como alternativa do envio da senha, podemos colocá-la ao final da linha de comando, veja o exemplo que segue:

<div class="wp_codebox">
  <table>
    <tr id="p12922">
      <td class="code" id="p129code22">
        <pre class="bash" style="font-family:monospace;">smbclient <span style="color: #000000; font-weight: bold;">//</span>192.168.12.201<span style="color: #000000; font-weight: bold;">/</span>teste <span style="color: #660033;">-U</span> usuario <span style="color: #660033;">-c</span> <span style="color: #ff0000;">"put /etc/hosts hosts;"</span> senha</pre>
      </td>
    </tr>
  </table>
</div>