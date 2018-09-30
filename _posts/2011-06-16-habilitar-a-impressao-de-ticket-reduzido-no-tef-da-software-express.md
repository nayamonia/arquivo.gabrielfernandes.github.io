---
id: 12
title: Habilitar a impressão de ticket reduzido no TEF
date: 2011-06-16T13:00:16+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=12
permalink: /2011/06/16/habilitar-a-impressao-de-ticket-reduzido-no-tef-da-software-express/
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
  - automação
tags:
  - reduzido
  - tef
  - ticket
---
Para habilitar a impressão da primeira via do cliente no SITEF da Software Express que passará a imprimir está via do cliente diretamente no campo mensagem publicitária do cupom fiscal é bem simples, vejamos o que devemos fazer:

1- Localizar a pasta da instalação do SITEF e entre na pasta **\Sitef\CONFIG**;

2- Editar o arquivo **sitefcfg.ini** da pasta **\Sitef\CONFIG**, use qualquer programa de edição de textos para isto, como por exemplo o notepad.exe;

<!--more-->

3- Procure neste arquivo a seção de configuração para a loja que desejamos habilitar o recurso, para identificar uma seção procure por **[XXXXXXXX]** onde **XXXXXXXX** é o número da empresa no SITEF, como por exemplo **[00000016]** para uma empresa de código 16. Caso não houver crie uma seção no arquivo, inserindo uma linha no final dele, como esta a seguir (exemplo para loja 16):

<div class="wp_codebox">
  <table>
    <tr id="p124">
      <td class="code" id="p12code4">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>00000016<span style="">&#93;</span></span></pre>
      </td>
    </tr>
  </table>
</div>

4- Logo abaixo a abertura da seção de configuração da loja no arquivo, insira uma linha com o parâmetro que habilita o TEF reduzido. O parâmetro capaz de ativar este recurso é o **HabilitaTicketReduzido** e para habilitá-lo basta colocá-lo igual a **1**. Como segue:

<div class="wp_codebox">
  <table>
    <tr id="p125">
      <td class="code" id="p12code5">
        <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>00000016<span style="">&#93;</span></span>
<span style="color: #000099;">HabilitaTicketReduzido</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">1</span></pre>
      </td>
    </tr>
  </table>
</div>

Pronto, agora basta reiniciarmos o serviço SITEF para que a configuração valha.

Boa sorte.