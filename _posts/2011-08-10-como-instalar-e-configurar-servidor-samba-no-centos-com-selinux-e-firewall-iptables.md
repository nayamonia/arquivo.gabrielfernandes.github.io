---
id: 402
title: Como instalar e configurar servidor Samba CentOS com SELinux e Firewall (iptables)
date: 2011-08-10T17:46:49+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=402
permalink: /2011/08/10/como-instalar-e-configurar-servidor-samba-no-centos-com-selinux-e-firewall-iptables/
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
image: /wp-content/uploads/2011/08/6029762026_6e73d8c472_b.jpg
categories:
  - linux
  - rede
  - segurança
tags:
  - centos
  - firewall
  - iptables
  - samba
  - selinux
---
Compartilhar uma pasta do Linux usando **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** é uma tarefa de extrema utilidade e relativamente simples, porém com o SELinux habilitado, precisamos dizer para ele que o **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** pode compartilhar aquele recurso na rede.

Este post dedica-se a mostrar como instalar, configurar o **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** e compartilhar uma pasta em uma instalação **<a href="http://www.gabrielfernandes.org/tag/centos/" target="_blank">CentOS</a>** com **Firewall** (**iptables**) e **SELinux** habilitados.

<!--more [CONTINUAR LENDO]-->

  * Instale o pacote **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** com o comando abaixo:
<div class="wp_codebox">
  <table>
    <tr id="p40266">
      <td class="code" id="p402code66">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> samba</pre>
      </td>
    </tr>
  </table>
</div>

  * Libera as portas de acesso do **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**, execute o comando abaixo:
<div class="wp_codebox">
  <table>
    <tr id="p40267">
      <td class="code" id="p402code67">
        <pre class="bash" style="font-family:monospace;">system-config-securitylevel-tui</pre>
      </td>
    </tr>
  </table>
</div>

Uma tela como esta a seguir, deve ser exibida, nela use o **TAB** para selecionar **Personalizar** e pressione **[Enter]**:
  
[<img src="https://i1.wp.com/farm7.staticflickr.com/6061/6029762026_6e73d8c472_z.jpg?resize=640%2C494&#038;ssl=1" alt="Liberar Samba 1 - system-config-securitylevel-tui " width="640" height="494" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6029762026/)

Ao selecionar **Personalizar** você deve estar em uma tela como esta abaixo, nela use o **TAB** para chegar até a opção **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** no campo **Permitir entrada**, depois use a **Barra de Espaço** para marcar **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6150/6029762014_dff7284130_z.jpg?resize=640%2C496&#038;ssl=1" alt="Liberar Samba 2 - system-config-securitylevel-tui " width="640" height="496" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/6029762014/)

Para sair e salvar, use **TAB** e selecione **OK**, que você retornará para a tela anterior e nela selecione **OK** novamente para confirmar a configuração e sair.

  * Configurar para iniciar automaticamente o servidor **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**, para tal execute:
<div class="wp_codebox">
  <table>
    <tr id="p40268">
      <td class="code" id="p402code68">
        <pre class="bash" style="font-family:monospace;">chkconfig <span style="color: #660033;">--level</span> <span style="color: #000000;">2345</span> smb on</pre>
      </td>
    </tr>
  </table>
</div>

  * Agora vamos definir os parâmetros **workgroup**, **server string** e **security** <del datetime="2011-08-10T19:19:56+00:00">este último já deve estar correto</del> no arquivo de configuração do **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**. Para isto execute o comando abaixo:
<div class="wp_codebox">
  <table>
    <tr id="p40269">
      <td class="code" id="p402code69">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>samba<span style="color: #000000; font-weight: bold;">/</span>smb.conf</pre>
      </td>
    </tr>
  </table>
</div>

Os valores dos parâmetros citados devem ficar parecidos com estes abaixo, claro com exceção para o **workgroup**, onde você deve inserir o nome do seu grupo de trabalho e o **server string** a descrição do servidor:

<div class="wp_codebox">
  <table>
    <tr id="p40270">
      <td class="code" id="p402code70">
        <pre class="bash" style="font-family:monospace;">workgroup = COMPOSTA
server string = Servidor Loja <span style="color: #000000;">90</span>
security = user</pre>
      </td>
    </tr>
  </table>
</div>

<del datetime="2011-08-10T20:21:52+00:00">Para sair do <strong>vi</strong> e salvar, use <strong>:wq</strong>.</del>

  * Continuando a configuração, vamos criar uma pasta para compartilhar, no exemplo a pasta chama-se **path_comum**, você pode usar o nome que desejar:
<div class="wp_codebox">
  <table>
    <tr id="p40271">
      <td class="code" id="p402code71">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">mkdir</span> <span style="color: #000000; font-weight: bold;">/</span>path_comum</pre>
      </td>
    </tr>
  </table>
</div>

  * Agora crie um usuário para acessar esta pasta, este usuário e senha serão utilizados para permitir o acesso ao compartilhamento quando este for acessado pela rede: <div class="wp_codebox">
      <table>
        <tr id="p40272">
          <td class="code" id="p402code72">
            <pre class="bash" style="font-family:monospace;">adduser <span style="color: #660033;">-M</span> zanthus</pre>
          </td>
        </tr>
      </table>
    </div>
    
    Defina uma senha para o sistema:
    
    <div class="wp_codebox">
      <table>
        <tr id="p40273">
          <td class="code" id="p402code73">
            <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">passwd</span> zanthus</pre>
          </td>
        </tr>
      </table>
    </div>
    
    Defina uma senha para o **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**, sugiro usar a mesma do sistema:
    
    <div class="wp_codebox">
      <table>
        <tr id="p40274">
          <td class="code" id="p402code74">
            <pre class="bash" style="font-family:monospace;">smbpasswd <span style="color: #660033;">-a</span> zanthus</pre>
          </td>
        </tr>
      </table>
    </div>
    
      * Ajustando algumas permissões da pasta no sistema de arquivos:
    <div class="wp_codebox">
      <table>
        <tr id="p40275">
          <td class="code" id="p402code75">
            <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chmod</span> <span style="color: #660033;">-Rf</span> <span style="color: #000000;">765</span> <span style="color: #000000; font-weight: bold;">/</span>path_comum</pre>
          </td>
        </tr>
      </table>
    </div>
    
    <div class="wp_codebox">
      <table>
        <tr id="p40276">
          <td class="code" id="p402code76">
            <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chown</span> <span style="color: #660033;">-R</span> zanthus.zanthus <span style="color: #000000; font-weight: bold;">/</span>path_comum</pre>
          </td>
        </tr>
      </table>
    </div>
    
      * Agora permissões do SELinux:
    Grava a configuração do contexto:
    
    <div class="wp_codebox">
      <table>
        <tr id="p40277">
          <td class="code" id="p402code77">
            <pre class="bash" style="font-family:monospace;">semanage fcontext <span style="color: #660033;">-a</span> <span style="color: #660033;">-t</span> samba_share_t <span style="color: #ff0000;">"/path_comum(/.*)?"</span></pre>
          </td>
        </tr>
      </table>
    </div>
    
    Faz valer as regras imediatamente:
    
    <div class="wp_codebox">
      <table>
        <tr id="p40278">
          <td class="code" id="p402code78">
            <pre class="bash" style="font-family:monospace;">chcon <span style="color: #660033;">-t</span> samba_share_t <span style="color: #000000; font-weight: bold;">/</span>path_comum<span style="color: #000000; font-weight: bold;">/</span></pre>
          </td>
        </tr>
      </table>
    </div>
    
      * Volte a editar o arquivo de configuração do **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** para incluir no final do arquivo os parâmetros de compartilhamento da pasta. Insira o conteudo abaixo no final do arquivo smb.conf: 
        Para editar utilize:
        
        <div class="wp_codebox">
          <table>
            <tr id="p40279">
              <td class="code" id="p402code79">
                <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>samba<span style="color: #000000; font-weight: bold;">/</span>smb.conf</pre>
              </td>
            </tr>
          </table>
        </div>
        
        Insira as linhas no final do arquivo:
        
        <div class="wp_codebox">
          <table>
            <tr id="p40280">
              <td class="code" id="p402code80">
                <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>path_comum<span style="color: #7a0874; font-weight: bold;">&#93;</span>
        comment = Area comum dos PDVs Zanthus
        path = <span style="color: #000000; font-weight: bold;">/</span>path_comum
        writeable = <span style="color: #c20cb9; font-weight: bold;">yes</span>
        valid <span style="color: #c20cb9; font-weight: bold;">users</span> = zanthus
        public = no
        printable = no
        create mask = 0765</pre>
              </td>
            </tr>
          </table>
        </div>
        
          * Por fim, inicie o serviço do **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>**:
        <div class="wp_codebox">
          <table>
            <tr id="p40281">
              <td class="code" id="p402code81">
                <pre class="bash" style="font-family:monospace;">service smb start</pre>
              </td>
            </tr>
          </table>
        </div></ul> 
        
        Feito, se tudo correu bem você deve estar com um servidor **<a href="http://www.gabrielfernandes.org/tag/samba/" target="_blank">Samba</a>** funcionando de modo seguro com um compartilhamento liberado para um determinado usuário.
        
        Vídeo True Post.
  
        Assista ao vídeo que testa este tutorial.
        
        <div class="jetpack-video-wrapper">
          <span class="embed-youtube" style="text-align:center; display: block;"></span>
        </div>