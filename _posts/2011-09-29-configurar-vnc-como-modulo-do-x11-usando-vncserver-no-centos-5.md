---
id: 820
title: Configurar VNC como módulo do X11 usando vncserver no CentOS 5
date: 2011-09-29T17:00:16+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=820
permalink: /2011/09/29/configurar-vnc-como-modulo-do-x11-usando-vncserver-no-centos-5/
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
image: /wp-content/uploads/2011/09/vncOSXvnc.jpg
categories:
  - automação
  - linux
  - shell
tags:
  - centos
  - módulo
  - vnc
  - vncserver
  - x11
---
Para quem necessita acessar a console gráfica (X Window) do **CentOS 5** e não quer instalar nenhum programa de terceiros, como o conhecido _x11vnc_, pode ativar o **VNC** como um **módulo do X11** e utilizar o **vncserver** que acompanha no pacote padrão da instalação do **CentOS 5**.

Desta forma sempre que o ambiente gráfico for iniciado a conexão **VNC** também estará disponível.

Para configurar este recurso, siga os procedimentos aqui descritos:
  
<!--more [CONTINUAR LENDO]-->

  * Instale o **vncserver**:
<div class="wp_codebox">
  <table>
    <tr id="p820144">
      <td class="code" id="p820code144">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> vnc-server</pre>
      </td>
    </tr>
  </table>
</div>

  * Edite o arquivo de configuração do **X11** no **CentOS 5**, podemos encontrá-lo no local _/etc/X11_, seu nome é _xorg.conf_. Insira na seção _&#8220;Module&#8221;_ &#8211; ou crie esta seção se não existir &#8211; a linha _Load &#8220;vnc&#8221;_ para carregar o módulo **VNC** na inicialização do _X Window_, como no exemplo que segue:
<div class="wp_codebox">
  <table>
    <tr id="p820145">
      <td class="code" id="p820code145">
        <pre class="bash" style="font-family:monospace;">Section <span style="color: #ff0000;">"Module"</span>
  Load <span style="color: #ff0000;">"vnc"</span>
EndSection</pre>
      </td>
    </tr>
  </table>
</div>

  * Quanto a segurança podemos ter acesso restrito com senha ou sem senha:
Para **acesso sem restrição de senha**. Basta inserir a linha abaixo na seção de configuração &#8220;_Screen_&#8220;, como segue:</p> 

<div class="wp_codebox">
  <table>
    <tr id="p820146">
      <td class="code" id="p820code146">
        <pre class="bash" style="font-family:monospace;">   Option <span style="color: #ff0000;">"SecurityTypes"</span> <span style="color: #ff0000;">"None"</span></pre>
      </td>
    </tr>
  </table>
</div>

Para **acesso restrito com senha**. Precisa-se além da inserção destas linhas abaixo na seção de configuração &#8220;_Screen_&#8220;, um cadastro de senha. Segue:

<div class="wp_codebox">
  <table>
    <tr id="p820147">
      <td class="code" id="p820code147">
        <pre class="bash" style="font-family:monospace;">   Option <span style="color: #ff0000;">"SecurityTypes"</span> <span style="color: #ff0000;">"VncAuth"</span> 
   Option <span style="color: #ff0000;">"UserPasswdVerifier"</span> <span style="color: #ff0000;">"VncAuth"</span> 
   Option <span style="color: #ff0000;">"PasswordFile"</span> <span style="color: #ff0000;">"/root/.vnc/passwd"</span></pre>
      </td>
    </tr>
  </table>
</div>

Crie uma senha para o usuário root no **VNC** com o comando abaixo, para o caso da configuração com acesso restrito por senha:

<div class="wp_codebox">
  <table>
    <tr id="p820148">
      <td class="code" id="p820code148">
        <pre class="bash" style="font-family:monospace;">vncpasswd</pre>
      </td>
    </tr>
  </table>
</div>

  * Para finalizar reinicie o ambiente gráfico ou o computador.

> **Dica:** Lembre-se de liberar a porta 5900 se estiver com o Firewall ativo.