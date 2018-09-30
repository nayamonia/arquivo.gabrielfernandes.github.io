---
id: 2148
title: Como instalar e configurar ZRetag da Zanthus integrado ao RMS da Totvs
date: 2015-07-02T09:29:23+00:00
author: Gabriel Fernandes
layout: page
guid: http://www.gabrielfernandes.org/?page_id=2148
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
image: /wp-content/uploads/2015/07/zanthusxrms.png
---
O ZRetag é um módulo para integração entre os sistemas Zanthus e RMS, mais específico para integrar com o CRM da RMS, agora da Totvs.

Para instalar siga os passos:

  1. Copie o executável ZRetag.exe na pasta c:\Zanthus\Zeus\ZRetag (se não existir, crie antes de copiar).
  2. Crie o arquivo ZRetag.ini na pasta &#8220;system&#8221; do sistema, por exemplo &#8220;c:\Windows\system32&#8221; (ou equivalente) com os parâmetros: <div class="wp_codebox">
      <table>
        <tr id="p2148287">
          <td class="code" id="p2148code287">
            <pre class="ini" style="font-family:monospace;"><span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>RETAG<span style="">&#93;</span></span>
<span style="color: #000099;">ATIVO</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">1 </span>
#Ver campo A00DMIA em M001.PDF
<span style="color: #000099;">A00DMIA</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">1</span>
#Porta de comunicação
<span style="color: #000099;">PORTATCP</span><span style="color: #000066; font-weight:bold;">=</span><span style="color: #660066;">12750</span>
&nbsp;
<span style="color: #000066; font-weight:bold;"><span style="">&#91;</span>RMS<span style="">&#93;</span></span>
#Esta seção é opcional, se não informado deve-se executar o programa VCRMCONF.EXE da RMS para configurar.
<span style="color: #000099;">BDNome</span><span style="color: #000066; font-weight:bold;">=</span>
<span style="color: #000099;">BDUsuario</span><span style="color: #000066; font-weight:bold;">=</span>
<span style="color: #000099;">BDSenha</span><span style="color: #000066; font-weight:bold;">=</span></pre>
          </td>
        </tr>
      </table>
    </div>

  3. Registre as bibliotecas (DLLs) RMSRMSFUNCOESCLIENT.DLL, VCRMCOMM.dll, VCRMSTEF.DLL e VFIAPDVN.DLL da RMS, com o comando: <div class="wp_codebox">
      <table>
        <tr id="p2148288">
          <td class="code" id="p2148code288">
            <pre class="bash" style="font-family:monospace;">regsvr32 PATH_COMPLETO_BIBLIOTECA</pre>
          </td>
        </tr>
      </table>
    </div>

  4. Instale o ZRetag como um serviço do Windows, na linha de comando digite: <div class="wp_codebox">
      <table>
        <tr id="p2148289">
          <td class="code" id="p2148code289">
            <pre class="bash" style="font-family:monospace;">ZRetag.exe <span style="color: #000000; font-weight: bold;">/</span><span style="color: #c20cb9; font-weight: bold;">install</span></pre>
          </td>
        </tr>
      </table>
    </div>

  5. Se tudo correu bem, inicie o serviço no painel de controle do Windows e seja feliz.

* Este post foi baseado no arquivo LEIAME que acompanha o pacote.