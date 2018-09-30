---
id: 1173
title: Como criar uma máquina virtual no servidor Xen Centos 5 e instalar o CentOS pela rede
date: 2012-06-04T14:19:44+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1173
permalink: /2012/06/04/como-criar-e-instalar-o-centos-em-uma-maquina-virtual-no-servidor-xen-centos-5/
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
image: /wp-content/uploads/2012/06/XenPanda.jpg
categories:
  - imagens
  - linux
  - shell
  - virtualização
tags:
  - centos
  - máquina virtual
  - servidor
  - xen
---
Se você acabou de instalar o **CentOS 5** com a opção Virtualização marcada, você estará apto a criar suas próprias máquinas virtuais **CentOS** seguindo as orientações a seguir:

Obtenha as imagens do Kernel **Xen** para a instalação do **CentOS** pelo console da **máquina virtual Xen** e copie para a pasta _/boot_:

<div class="wp_codebox">
  <table>
    <tr id="p1173175">
      <td class="code" id="p1173code175">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">wget</span> http:<span style="color: #000000; font-weight: bold;">//</span>mirror.centos.org<span style="color: #000000; font-weight: bold;">/</span>centos<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span>os<span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>images<span style="color: #000000; font-weight: bold;">/</span>xen<span style="color: #000000; font-weight: bold;">/</span>initrd.img</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p1173176">
      <td class="code" id="p1173code176">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">cp</span> initrd.img <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #000000; font-weight: bold;">/</span>initrd-xen-install</pre>
      </td>
    </tr>
  </table>
</div>

<!--more [CONTINUAR LENDO]-->

<div class="wp_codebox">
  <table>
    <tr id="p1173177">
      <td class="code" id="p1173code177">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">wget</span> http:<span style="color: #000000; font-weight: bold;">//</span>mirror.centos.org<span style="color: #000000; font-weight: bold;">/</span>centos<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span>os<span style="color: #000000; font-weight: bold;">/</span>i386<span style="color: #000000; font-weight: bold;">/</span>images<span style="color: #000000; font-weight: bold;">/</span>xen<span style="color: #000000; font-weight: bold;">/</span>vmlinuz</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p1173178">
      <td class="code" id="p1173code178">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">cp</span> vmlinuz <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #000000; font-weight: bold;">/</span>vmlinuz-xen-install</pre>
      </td>
    </tr>
  </table>
</div>

Se você não tiver uma imagem de disco rígido para utilizar nesta instalação, crie uma com o comando abaixo. Note que este comando cria um disco de 40Gb com crescimento dinamico:

<div class="wp_codebox">
  <table>
    <tr id="p1173179">
      <td class="code" id="p1173code179">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">dd</span> <span style="color: #007800;">if</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>zero <span style="color: #007800;">of</span>=bapi.disk.img <span style="color: #007800;">bs</span>=1M <span style="color: #007800;">seek</span>=<span style="color: #000000;">40960</span> <span style="color: #007800;">count</span>=<span style="color: #000000;"></span></pre>
      </td>
    </tr>
  </table>
</div>

Crie o arquivo de configuração &#8211; nome do arquivo: _bapi.conf.linux-install_ &#8211; para iniciar a **máquina virtual** no &#8220;modo de instalação&#8221;, lembre-se este arquivo é temporário e apenas para efetuar a instalação, mas a frente veremos como criar um arquivo definitivo para a **máquina virtual**, segue:

<div class="wp_codebox">
  <table>
    <tr id="p1173180">
      <td class="code" id="p1173code180">
        <pre class="bash" style="font-family:monospace;">kernel = <span style="color: #ff0000;">"/boot/vmlinuz-xen-install"</span>
ramdisk = <span style="color: #ff0000;">"/boot/initrd-xen-install"</span>
memory = <span style="color: #000000;">512</span>
name = <span style="color: #ff0000;">"bapi"</span>
vif = <span style="color: #7a0874; font-weight: bold;">&#91;</span> <span style="color: #ff0000;">'bridge=xenbr0'</span> <span style="color: #7a0874; font-weight: bold;">&#93;</span>
disk = <span style="color: #7a0874; font-weight: bold;">&#91;</span> <span style="color: #ff0000;">'tap:aio:/vms/bapi/bapi.disk.img,xvda,w'</span>, <span style="color: #7a0874; font-weight: bold;">&#93;</span>
on_reboot   = <span style="color: #ff0000;">'destroy'</span>
on_crash    = <span style="color: #ff0000;">'destroy'</span></pre>
      </td>
    </tr>
  </table>
</div>

> É possível _encurtar_ todo o processo de instalação usando arquivos ks, não entrarei em detalhes sobre este arquivo, pois é assunto para pelo menos uma postagem completa. Mas os _experts_ podem adicionar a linha _extra_ e colocar parâmetros como modo de instalação e endereço do arquivo ks, exemplo:
> 
> <div class="wp_codebox">
>   <table>
>     <tr id="p1173181">
>       <td class="code" id="p1173code181">
>         <pre class="bash" style="font-family:monospace;">extra = <span style="color: #ff0000;">"text ks=http://192.168.254.95/test/minimal-ks.cfg"</span></pre>
>       </td>
>     </tr>
>   </table>
> </div>

Para iniciar a **máquina virtual**, execute o comando create do **servidor Xen** informando o caminho completo do arquivo de configuração com o comando abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p1173182">
      <td class="code" id="p1173code182">
        <pre class="bash" style="font-family:monospace;">xm create bapi.conf.linux-install</pre>
      </td>
    </tr>
  </table>
</div>

A saída deve ser algo assim:

<div class="wp_codebox">
  <table>
    <tr id="p1173183">
      <td class="code" id="p1173code183">
        <pre class="bash" style="font-family:monospace;">Using config <span style="color: #c20cb9; font-weight: bold;">file</span> <span style="color: #ff0000;">"./bapi.conf.linux-install"</span>.
Started domain bapi</pre>
      </td>
    </tr>
  </table>
</div>

Agora vamos acessar a console da **máquina virtual** e iniciar a instalação do **CentOS**, para isto use o comando _console_ do **servidor Xen** como segue:

<div class="wp_codebox">
  <table>
    <tr id="p1173184">
      <td class="code" id="p1173code184">
        <pre class="bash" style="font-family:monospace;">xm console bapi</pre>
      </td>
    </tr>
  </table>
</div>

Se tudo correu bem até aqui, você deve visualizar uma tela como esta que segue, referente a seleção do idioma da instalação.

> Esta é uma escolha do usuário, eu escolhi **Portuguese(Brazilian)**.

<div class="wp_codebox">
  <table>
    <tr id="p1173185">
      <td class="code" id="p1173code185">
        <pre class="bash" style="font-family:monospace;">Welcome to CentOS                                                               
&nbsp;
                   +---------+ Choose a Language +---------+                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span> What language would you like to use   <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span> during the installation process?      <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Northern Sotho         ^        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Oriya                  :        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Polish                 :        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Portuguese             :        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Portuguese<span style="color: #7a0874; font-weight: bold;">&#40;</span>Brazilian<span style="color: #7a0874; font-weight: bold;">&#41;</span>  <span style="color: #666666; font-style: italic;">#        |                    </span>
                   <span style="color: #000000; font-weight: bold;">|</span>       Punjabi                :        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Russian                :        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>       Serbian                v        <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                +----+                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                +----+                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   +---------------------------------------+                    
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> between elements  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Space<span style="color: #000000; font-weight: bold;">&gt;</span> selects <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> next <span style="color: #c20cb9; font-weight: bold;">screen</span></pre>
      </td>
    </tr>
  </table>
</div>

Após a seleção do idioma já podemos informar qual será o método de instalação.
  
Aqui usaremos o _método **HTTP**_, que necessita conexão com a internet, mas pelo menos não precisamos baixar todo o **CentOS** antes de instalar, desta forma serão baixados apenas os pacotes que serão instalados.

<div class="wp_codebox">
  <table>
    <tr id="p1173186">
      <td class="code" id="p1173code186">
        <pre class="bash" style="font-family:monospace;">&nbsp;
Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
                       +---+ Método de Instalação +-+                           
                       <span style="color: #000000; font-weight: bold;">|</span>                               <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span> Que tipo de mídia contém os <span style="color: #000000; font-weight: bold;">|</span>                          
                       <span style="color: #000000; font-weight: bold;">|</span> pacotes a serem instalados?   <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>                               <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>         CDROM Local           <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>         Disco rígido         <span style="color: #000000; font-weight: bold;">|</span>                         
                       <span style="color: #000000; font-weight: bold;">|</span>         Imagem NFS            <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>         FTP                   <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>         HTTP                  <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>                               <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>   +----+       +--------+     <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>   <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>       <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>     <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>   +----+       +--------+     <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>                               <span style="color: #000000; font-weight: bold;">|</span>                        
                       <span style="color: #000000; font-weight: bold;">|</span>                               <span style="color: #000000; font-weight: bold;">|</span>                        
                       +-------------------------------+                        
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Já que precisamos da internet, obviamente precisaremos configurar um endereço de IP para a instalação do **CentOS**.

> A todo momento tomamos decisões, aqui mais uma. Pois entre eu e você, não há ninguém melhor para tomar esta decisão do que você mesmo. Então fique a vontade para escolher entre IP estático **(Manual configuration)** ou dinâmico **(Dynamic IP configuration (DHCP))**.

<div class="wp_codebox">
  <table>
    <tr id="p1173187">
      <td class="code" id="p1173code187">
        <pre class="bash" style="font-family:monospace;">&nbsp;
Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
             +---------------+ Configurar TCP<span style="color: #000000; font-weight: bold;">/</span>IP +----------------+             
             <span style="color: #000000; font-weight: bold;">|</span>                                                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000; font-weight: bold;">*</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> Habilitar suporte para IPv4                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #7a0874; font-weight: bold;">&#40;</span> <span style="color: #7a0874; font-weight: bold;">&#41;</span> Dynamic IP configuration <span style="color: #7a0874; font-weight: bold;">&#40;</span>DHCP<span style="color: #7a0874; font-weight: bold;">&#41;</span>         <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000; font-weight: bold;">*</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> Manual configuration                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>                                                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span> <span style="color: #7a0874; font-weight: bold;">&#93;</span> Habilitar suporte para IPv6                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000; font-weight: bold;">*</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> Automatic neighbor discovery <span style="color: #7a0874; font-weight: bold;">&#40;</span>RFC <span style="color: #000000;">2461</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #7a0874; font-weight: bold;">&#40;</span> <span style="color: #7a0874; font-weight: bold;">&#41;</span> Dynamic IP configuration <span style="color: #7a0874; font-weight: bold;">&#40;</span>DHCP<span style="color: #7a0874; font-weight: bold;">&#41;</span>         <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #7a0874; font-weight: bold;">&#40;</span> <span style="color: #7a0874; font-weight: bold;">&#41;</span> Manual configuration                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>                                                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>         +----+                 +--------+          <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>         <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>                 <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>          <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>         +----+                 +--------+          <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>                                                    <span style="color: #000000; font-weight: bold;">|</span>             
             <span style="color: #000000; font-weight: bold;">|</span>                                                    <span style="color: #000000; font-weight: bold;">|</span>             
             +----------------------------------------------------+             
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Se você também definiu IP fixo uma tela para a configuração do IP, como esta abaixo deve estar sendo exibida.
  
Configure o IP e siga em frente.

<div class="wp_codebox">
  <table>
    <tr id="p1173188">
      <td class="code" id="p1173code188">
        <pre class="bash" style="font-family:monospace;">&nbsp;
Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
        +--------------+ Configuração manual de TCP<span style="color: #000000; font-weight: bold;">/</span>IP +-------------+          
        <span style="color: #000000; font-weight: bold;">|</span>                                                              <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span> Entre o endereço e o prefixo IPv4 e<span style="color: #000000; font-weight: bold;">/</span>ou IPv6 <span style="color: #7a0874; font-weight: bold;">&#40;</span>endereço <span style="color: #000000; font-weight: bold;">/</span>    <span style="color: #000000; font-weight: bold;">|</span>          
        <span style="color: #000000; font-weight: bold;">|</span> prefixo<span style="color: #7a0874; font-weight: bold;">&#41;</span>.  Para IPv4, a notação ponto-decimal ou o estilo  <span style="color: #000000; font-weight: bold;">|</span>          
        <span style="color: #000000; font-weight: bold;">|</span> CIDR são aceitos. Os campos de gateway e nome de servidor   <span style="color: #000000; font-weight: bold;">|</span>         
        <span style="color: #000000; font-weight: bold;">|</span> devem conter endereços IPv4 ou IPv6 válidos.               <span style="color: #000000; font-weight: bold;">|</span>          
        <span style="color: #000000; font-weight: bold;">|</span>                                                              <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span> Endereço IPv4:    _192.168.254.111 <span style="color: #000000; font-weight: bold;">/</span> _24_____________       <span style="color: #000000; font-weight: bold;">|</span>         
        <span style="color: #000000; font-weight: bold;">|</span> Gateway:           192.168.254.241__________________________ <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span> Servidor de Nomes: 192.168.254.241__________________________ <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>                                                              <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>            +----+                      +--------+            <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>            <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>                      <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>            <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>            +----+                      +--------+            <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>                                                              <span style="color: #000000; font-weight: bold;">|</span>        
        <span style="color: #000000; font-weight: bold;">|</span>                                                              <span style="color: #000000; font-weight: bold;">|</span>        
        +--------------------------------------------------------------+        
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Este é um dos momentos mais interessantes deste tipo de instalação, pois agora devemos definir o local na web ou endereço web de onde encontra-se o repositório de arquivos do **CentOS**, pois como foi dito anteriormente a instalação HTTP é feita on-line e precisa de conexão com a internet para baixar os arquivos da instalação e os pacotes que serão instalados.

> O **Nome do servidor Web** e o **Diretório CentOS** definidos abaixo, indicam que vamos instalar a última versão do **CentOS 5**.

<div class="wp_codebox">
  <table>
    <tr id="p1173189">
      <td class="code" id="p1173code189">
        <pre class="bash" style="font-family:monospace;">&nbsp;
Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
               +------------+ Configuração <span style="color: #000000; font-weight: bold;">do</span> HTTP +----------+                 
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Por favor insira <span style="color: #c20cb9; font-weight: bold;">as</span> seguintes informações:   <span style="color: #000000; font-weight: bold;">|</span>                 
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>     o o nome ou o endereço IP <span style="color: #000000; font-weight: bold;">do</span> servidor Web <span style="color: #000000; font-weight: bold;">|</span>                
               <span style="color: #000000; font-weight: bold;">|</span>     o o diretório neste servidor que contém o<span style="color: #000000; font-weight: bold;">|</span>                 
               <span style="color: #000000; font-weight: bold;">|</span>       CentOS para a sua arquitetura            <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Nome <span style="color: #000000; font-weight: bold;">do</span> servidor Web: mirror.centos.org_______ <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Diretório CentOS:    _centos-<span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">5</span><span style="color: #000000; font-weight: bold;">/</span>os<span style="color: #000000; font-weight: bold;">/</span>i386_____ <span style="color: #000000; font-weight: bold;">|</span>                
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        +----+               +--------+         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>               <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        +----+               +--------+         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               +------------------------------------------------+               
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Neste momento o instalador vai tentar efetuar o download das imagens de instalação via internet, uma tela como esta a seguir deve ser exibida enquanto ocorre o download.

<div class="wp_codebox">
  <table>
    <tr id="p1173190">
      <td class="code" id="p1173code190">
        <pre class="bash" style="font-family:monospace;">&nbsp;
Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
    +-----------------------------+ Obtendo +------------------------------+    
    <span style="color: #000000; font-weight: bold;">|</span>                                                                      <span style="color: #000000; font-weight: bold;">|</span>    
    <span style="color: #000000; font-weight: bold;">|</span> Obtendo images<span style="color: #000000; font-weight: bold;">/</span>stage2.img...                                         <span style="color: #000000; font-weight: bold;">|</span>    
    <span style="color: #000000; font-weight: bold;">|</span>                                                                      <span style="color: #000000; font-weight: bold;">|</span>    
    +----------------------------------------------------------------------+    
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Se após algum tempo, retornar uma mensagem como esta que segue abaixo, é porque você não está usando a última versão do **CentOS**, pois como dito, o endereço _centos-5/5/os/i386_ aponta para a versão mais recente.

<div class="wp_codebox">
  <table>
    <tr id="p1173191">
      <td class="code" id="p1173code191">
        <pre class="bash" style="font-family:monospace;">Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
                   +---------------+ Erro +----------------+                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span> A árvore de instalação CentOS deste<span style="color: #000000; font-weight: bold;">|</span>                       
                   <span style="color: #000000; font-weight: bold;">|</span> diretório não parece combinar com a <span style="color: #000000; font-weight: bold;">|</span>                      
                   <span style="color: #000000; font-weight: bold;">|</span> mídia de inicialização.            <span style="color: #000000; font-weight: bold;">|</span>                       
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                +----+                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                +----+                 <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   <span style="color: #000000; font-weight: bold;">|</span>                                       <span style="color: #000000; font-weight: bold;">|</span>                    
                   +---------------------------------------+                    
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Neste caso você vai precisar especificar a versão que você instalou, por exemplo para o **CentOS 5.5** o endereço é _5.5/os/i386_ no site _vault.centos.org_.

> Versões &#8220;obsoletas&#8221; do **CentOS** são retiradas da arvore de diretório do repositório principal, então se a versão desejada não está mais no <a href="http://mirror.centos.org" target="_blank">http://mirror.centos.org</a> tenha certeza que não haverá mais atualizações para ela. Mas se você sabe o que está fazendo pode obter estas versões em <a href="http://vault.centos.org" target="_blank">http://vault.centos.org</a>

> **Nome do servidor Web** e o **Diretório CentOS** definidos abaixo, indicam que vamos instalar uma versão específica do **CentOS**, no caso a versão 5.5

<div class="wp_codebox">
  <table>
    <tr id="p1173192">
      <td class="code" id="p1173code192">
        <pre class="bash" style="font-family:monospace;">Bem-vindo ao CentOS                                                             
&nbsp;
&nbsp;
               +------------+ Configuração <span style="color: #000000; font-weight: bold;">do</span> HTTP +----------+                 
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Por favor insira <span style="color: #c20cb9; font-weight: bold;">as</span> seguintes informações:   <span style="color: #000000; font-weight: bold;">|</span>                 
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>     o o nome ou o endereço IP <span style="color: #000000; font-weight: bold;">do</span> servidor Web <span style="color: #000000; font-weight: bold;">|</span>                
               <span style="color: #000000; font-weight: bold;">|</span>     o o diretório neste servidor que contém o<span style="color: #000000; font-weight: bold;">|</span>                 
               <span style="color: #000000; font-weight: bold;">|</span>       CentOS para a sua arquitetura            <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Nome <span style="color: #000000; font-weight: bold;">do</span> servidor Web: vault.centos.org________ <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span> Diretório CentOS:    <span style="color: #000000;">5.5</span><span style="color: #000000; font-weight: bold;">/</span>os<span style="color: #000000; font-weight: bold;">/</span>i386_____________ <span style="color: #000000; font-weight: bold;">|</span>                
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        +----+               +--------+         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>               <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>        +----+               +--------+         <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               <span style="color: #000000; font-weight: bold;">|</span>                                                <span style="color: #000000; font-weight: bold;">|</span>               
               +------------------------------------------------+               
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> alterna seleção  <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>Espaço<span style="color: #000000; font-weight: bold;">&gt;</span> seleciona <span style="color: #000000; font-weight: bold;">|</span> <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> continuar</pre>
      </td>
    </tr>
  </table>
</div>

Agora para transformar esta experiência mais interessante ainda, vamos continuar a instalação pelo **VNC**. Ahh! Porque?

Além da comodidade da instalação via interface gráfica, novos usuários familiarizam-se melhor com a instalação em modo gráfico, caso contrário explicar todas as telas da instalação via texto, ia estender muito este post.

O que vamos fazer agora é ativar o **VNC** para que possamos continuar com a instalação de qualquer outro computador da rede usando o cliente de **VNC** de sua preferência, desta forma iniciaremos a instalação em modo gráfico.

> É hora de escolher novamente, se quiser seguir a sugestão do autor deste post, selecione **Iniciar VNC**

<div class="wp_codebox">
  <table>
    <tr id="p1173193">
      <td class="code" id="p1173code193">
        <pre class="bash" style="font-family:monospace;">&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
                +-------+ Você gostaria de usar o VNC? +-------+                
                <span style="color: #000000; font-weight: bold;">|</span>                                              <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>   O modo de instalação VNC oferece mais      <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>   funcionalidade <span style="color: #000000; font-weight: bold;">do</span> que o modo texto,        <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>   você tem certeza de quer usá-lo            <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>                                              <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>  +---------------------+   +-------------+   <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">|</span> Utilizar modo texto <span style="color: #000000; font-weight: bold;">|</span>   <span style="color: #000000; font-weight: bold;">|</span> Iniciar VNC <span style="color: #000000; font-weight: bold;">|</span>   <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>  +---------------------+   +-------------+   <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>                                              <span style="color: #000000; font-weight: bold;">|</span>                
                <span style="color: #000000; font-weight: bold;">|</span>                                              <span style="color: #000000; font-weight: bold;">|</span>                
                +----------------------------------------------+                
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> between elements   <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">&lt;</span>Space<span style="color: #000000; font-weight: bold;">&gt;</span> selects   <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> next <span style="color: #c20cb9; font-weight: bold;">screen</span></pre>
      </td>
    </tr>
  </table>
</div>

> De novo? É verdade, faça mais uma escolha, deseja que o **VNC** tenha senha ou não.

<div class="wp_codebox">
  <table>
    <tr id="p1173194">
      <td class="code" id="p1173code194">
        <pre class="bash" style="font-family:monospace;">&nbsp;
&nbsp;
&nbsp;
&nbsp;
                  +---------+ Configuração <span style="color: #000000; font-weight: bold;">do</span> VNC +---------+                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  Uma senha evitará que acessos não      <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  autorizados observem e monitorem o     <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  progresso de sua instalação.  Por      <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  favor , entre com uma senha para       <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  ser usada para a instalação            <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>   Senha:             ________________   <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>   Senha <span style="color: #7a0874; font-weight: bold;">&#40;</span>confirmar<span style="color: #7a0874; font-weight: bold;">&#41;</span>: ________________   <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  +----+   +-----------+   +--------+    <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">|</span> OK <span style="color: #000000; font-weight: bold;">|</span>   <span style="color: #000000; font-weight: bold;">|</span> Sem senha <span style="color: #000000; font-weight: bold;">|</span>   <span style="color: #000000; font-weight: bold;">|</span> Voltar <span style="color: #000000; font-weight: bold;">|</span>    <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>  +----+   +-----------+   +--------+    <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  <span style="color: #000000; font-weight: bold;">|</span>                                         <span style="color: #000000; font-weight: bold;">|</span>                   
                  +-----------------------------------------+                   
&nbsp;
&nbsp;
  <span style="color: #000000; font-weight: bold;">&lt;</span>Tab<span style="color: #000000; font-weight: bold;">&gt;/&lt;</span>Alt-Tab<span style="color: #000000; font-weight: bold;">&gt;</span> between elements   <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">&lt;</span>Space<span style="color: #000000; font-weight: bold;">&gt;</span> selects   <span style="color: #000000; font-weight: bold;">|</span>  <span style="color: #000000; font-weight: bold;">&lt;</span>F12<span style="color: #000000; font-weight: bold;">&gt;</span> next <span style="color: #c20cb9; font-weight: bold;">screen</span></pre>
      </td>
    </tr>
  </table>
</div>

Ufa, belezas. Agora uma mensagem como esta a seguir deve ser exibida no console da máquina.
  
Observe as informações contidas nela, principalmente aquele que diz **Por favor conecte a 192.168.254.111:1 para iniciar a instalação&#8230;**, ou seja, o endereço de IP:Porta que é exibido nesta tela é o que você deve utilizar para conectar com o cliente **VNC**.

> Só para deixar em pratos limpos, o IP 192.168.254.111 é o que usei nos testes para este post, o seu provavelmente será outro.

<div class="wp_codebox">
  <table>
    <tr id="p1173195">
      <td class="code" id="p1173code195">
        <pre class="bash" style="font-family:monospace;">Probing <span style="color: #000000; font-weight: bold;">for</span> video card:   Unable to probe                                      
Não foi encontrado hardware de vídeo, assumindo falta de monitor
Iniciando VNC...
O servidor VNC está rodando.
Por favor conecte a 192.168.254.111:<span style="color: #000000;">1</span> para iniciar a instalação...
Pressione <span style="color: #000000; font-weight: bold;">&lt;</span>enter<span style="color: #000000; font-weight: bold;">&gt;</span> para obter uma janela de comandos
Iniciando instalação gráfica...
XKB extension not present on :<span style="color: #000000;">1</span></pre>
      </td>
    </tr>
  </table>
</div>

> Já que **<q>Uma imagem vale mais que mil palavras!</q>**, vou deixar elas falarem por mim. Segue as imagens da instalação gráfica via **VNC**. Se preferir [clique aqui para avançar as imagens e continuar lendo](#continuaaqui), pois vem mais coisas legais por ai. 

<center>
  <br /> <a href="http://www.flickr.com/photos/nayamonia/7315574630/"><img src="https://i0.wp.com/farm8.staticflickr.com/7091/7315574630_07b0ab8580_z.jpg?resize=640%2C566&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="566" data-recalc-dims="1" /></a></p> 
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315574708/"><img src="https://i2.wp.com/farm9.staticflickr.com/8012/7315574708_676d0b333b_z.jpg?resize=640%2C566&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="566" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315574784/"><img src="https://i1.wp.com/farm8.staticflickr.com/7087/7315574784_aef7c3ed63_z.jpg?resize=640%2C563&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="563" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315574936/"><img src="https://i2.wp.com/farm8.staticflickr.com/7235/7315574936_796b6c17b9_z.jpg?resize=640%2C564&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="564" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315574998/"><img src="https://i1.wp.com/farm8.staticflickr.com/7223/7315574998_be18020f49_z.jpg?resize=640%2C563&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="563" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315575068/"><img src="https://i1.wp.com/farm8.staticflickr.com/7218/7315575068_b5e6327e1e_z.jpg?resize=640%2C565&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="565" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315575158/"><img src="https://i0.wp.com/farm9.staticflickr.com/8161/7315575158_1a4a93e635_z.jpg?resize=640%2C564&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="564" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315575308/"><img src="https://i1.wp.com/farm8.staticflickr.com/7211/7315575308_62ca61eff4_z.jpg?resize=640%2C562&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="562" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315575228/"><img src="https://i2.wp.com/farm9.staticflickr.com/8005/7315575228_d215baa4bc_z.jpg?resize=640%2C564&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="564" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315575406/"><img src="https://i0.wp.com/farm9.staticflickr.com/8025/7315575406_569ef1c79c_z.jpg?resize=640%2C566&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="566" data-recalc-dims="1" /></a>
  </p>
  
  <p>
    <a href="http://www.flickr.com/photos/nayamonia/7315574538/"><img src="https://i0.wp.com/farm9.staticflickr.com/8143/7315574538_56f47ac4ac_z.jpg?resize=640%2C563&#038;ssl=1" alt="Instalação CentOS por VNC" width="640" height="563" data-recalc-dims="1" /></a><br /> </center>
  </p>
  
  <p>
    <span id="continuaaqui"></span>
  </p>
  
  <p>
    Isto que você vê abaixo é a tela da console ao terminar a instalação pelo <strong>VNC</strong>.
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173196">
        <td class="code" id="p1173code196">
          <pre class="bash" style="font-family:monospace;">sending termination signals...done
sending <span style="color: #c20cb9; font-weight: bold;">kill</span> signals...done
disabling swap...
	<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>mapper<span style="color: #000000; font-weight: bold;">/</span>VolGroup00-LogVol01
unmounting filesystems...
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>runtime <span style="color: #000000; font-weight: bold;">done</span>
	disabling <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>loop0
	<span style="color: #000000; font-weight: bold;">/</span>proc <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>pts <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>sys <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>tmp<span style="color: #000000; font-weight: bold;">/</span>ramfs <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>selinux <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>boot <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>sys <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>proc<span style="color: #000000; font-weight: bold;">/</span>bus<span style="color: #000000; font-weight: bold;">/</span>usb <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>proc <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>selinux <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage<span style="color: #000000; font-weight: bold;">/</span>dev <span style="color: #000000; font-weight: bold;">done</span>
	<span style="color: #000000; font-weight: bold;">/</span>mnt<span style="color: #000000; font-weight: bold;">/</span>sysimage <span style="color: #000000; font-weight: bold;">done</span>
rebooting system
Restarting system.</pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    Terminada a instalação do <strong>CentOS</strong>, podemos criar o arquivo definitivo de inicialização da <strong>máquina virtual</strong>, ou seja, sem carregar as imagens de instalação do kernel <strong>Xen</strong>.
  </p>
  
  <p>
    Crie o arquivo de configuração &#8211; nome do arquivo: <em>bapi.conf.linux</em> &#8211; para iniciar a <strong>máquina virtual </strong>no &#8220;modo de normal&#8221;, segue:
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173197">
        <td class="code" id="p1173code197">
          <pre class="bash" style="font-family:monospace;"><span style="color: #007800;">bootloader</span>=<span style="color: #ff0000;">"/usr/bin/pygrub"</span>
memory = <span style="color: #000000;">1024</span>
name = <span style="color: #ff0000;">"bapi"</span>
vif = <span style="color: #7a0874; font-weight: bold;">&#91;</span> <span style="color: #ff0000;">'bridge=xenbr0'</span> <span style="color: #7a0874; font-weight: bold;">&#93;</span>
disk = <span style="color: #7a0874; font-weight: bold;">&#91;</span> <span style="color: #ff0000;">'tap:aio:/vms/bapi/bapi.disk.img,xvda,w'</span>, <span style="color: #7a0874; font-weight: bold;">&#93;</span>
extra = <span style="color: #ff0000;">"3"</span>
on_poweroff = <span style="color: #ff0000;">'destroy'</span>
on_reboot   = <span style="color: #ff0000;">'restart'</span>
on_crash    = <span style="color: #ff0000;">'restart'</span></pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    Para iniciar a <strong>máquina virtual</strong>, execute o comando <em>create</em> do <strong>servidor Xen</strong> informando o caminho completo do arquivo de configuração como no comando abaixo:
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173198">
        <td class="code" id="p1173code198">
          <pre class="bash" style="font-family:monospace;">xm create bapi.conf.linux</pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    A saída deve ser algo assim:
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173199">
        <td class="code" id="p1173code199">
          <pre class="bash" style="font-family:monospace;">Using config <span style="color: #c20cb9; font-weight: bold;">file</span> <span style="color: #ff0000;">"./bapi.conf.linux"</span>.
Using <span style="color: #000000; font-weight: bold;">&lt;</span>class <span style="color: #ff0000;">'grub.GrubConf.GrubConfigFile'</span><span style="color: #000000; font-weight: bold;">&gt;</span> to parse <span style="color: #000000; font-weight: bold;">/</span>grub<span style="color: #000000; font-weight: bold;">/</span>menu.lst
Started domain bapi</pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    Pronto, agora se tudo correu bem até aqui, já podemos acessar a console da <strong>máquina virtual</strong> instalada, a partir do comando:
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173200">
        <td class="code" id="p1173code200">
          <pre class="bash" style="font-family:monospace;">xm console bapi</pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    Se você executou o comando acima logo após ter dado o comando create, é provável que você veja mensagens do Kernel como estas abaixo, até o carregamento completo do sistema operacional e solicitação de login:
  </p>
  
  <p>
    <a href="#saidaqui">clique aqui para pular estas mensagens e continuar lendo</a>
  </p>
  
  <div class="wp_codebox">
    <table>
      <tr id="p1173201">
        <td class="code" id="p1173code201">
          <pre class="bash" style="font-family:monospace;">Linux version 2.6.18-<span style="color: #000000;">308</span>.el5xen <span style="color: #7a0874; font-weight: bold;">&#40;</span>mockbuild<span style="color: #000000; font-weight: bold;">@</span>builder10.centos.org<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #c20cb9; font-weight: bold;">gcc</span> version 4.1.2 <span style="color: #000000;">20080704</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Red Hat 4.1.2-<span style="color: #000000;">52</span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #666666; font-style: italic;">#1 SMP Tue Feb 21 21:26:03 EST 2012</span>
BIOS-provided physical RAM map:
 Xen: 0000000000000000 - 0000000040800000 <span style="color: #7a0874; font-weight: bold;">&#40;</span>usable<span style="color: #7a0874; font-weight: bold;">&#41;</span>
304MB HIGHMEM available.
727MB LOWMEM available.
NX <span style="color: #7a0874; font-weight: bold;">&#40;</span>Execute Disable<span style="color: #7a0874; font-weight: bold;">&#41;</span> protection: active
ACPI <span style="color: #000000; font-weight: bold;">in</span> unprivileged domain disabled
Built <span style="color: #000000;">1</span> zonelists.  Total pages: <span style="color: #000000;">264192</span>
Kernel <span style="color: #7a0874; font-weight: bold;">command</span> line:  ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 <span style="color: #007800;">console</span>=xvc0
Enabling fast FPU save and restore... done.
Enabling unmasked SIMD FPU exception support... done.
Initializing CPU<span style="color: #666666; font-style: italic;">#0</span>
CPU <span style="color: #000000;"></span> irqstacks, <span style="color: #007800;">hard</span>=c0784000 <span style="color: #007800;">soft</span>=c0764000
PID <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">4096</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">12</span>, <span style="color: #000000;">16384</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Xen reported: <span style="color: #000000;">2400.084</span> MHz processor.
Console: colour dummy device 80x25
Dentry cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">7</span>, <span style="color: #000000;">524288</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Inode-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">65536</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">6</span>, <span style="color: #000000;">262144</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Software IO TLB disabled
vmalloc area: ee000000-f4ffe000, maxmem 2d7fe000
Memory: 1024372k<span style="color: #000000; font-weight: bold;">/</span>1056768k available <span style="color: #7a0874; font-weight: bold;">&#40;</span>2212k kernel code, 23552k reserved, 1038k data, 184k init, 311304k highmem<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Checking <span style="color: #000000; font-weight: bold;">if</span> this processor honours the WP bit even <span style="color: #000000; font-weight: bold;">in</span> supervisor mode... Ok.
Calibrating delay using timer specific routine.. <span style="color: #000000;">6002.86</span> BogoMIPS <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #007800;">lpj</span>=<span style="color: #000000;">12005729</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
Security Framework v1.0.0 initialized
SELinux:  Initializing.
selinux_register_security:  Registering secondary module capability
Capability LSM initialized <span style="color: #c20cb9; font-weight: bold;">as</span> secondary
Mount-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">512</span>
CPU: L1 I cache: 32K, L1 D cache: 32K
CPU: L2 cache: 256K
CPU: L3 cache: 8192K
Checking <span style="color: #ff0000;">'hlt'</span> instruction... OK.
SMP alternatives: switching to UP code
Freeing SMP alternatives: 14k freed
Brought up <span style="color: #000000;">1</span> CPUs
checking <span style="color: #000000; font-weight: bold;">if</span> image is initramfs... it is
Freeing initrd memory: 7304k freed
Grant table initialized
NET: Registered protocol family <span style="color: #000000;">16</span>
Brought up <span style="color: #000000;">1</span> CPUs
PCI: setting up Xen PCI frontend stub
ACPI: Interpreter disabled.
Linux Plug and Play Support v0.97 <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> Adam Belay
pnp: PnP ACPI: disabled
xen_mem: Initialising balloon driver.
usbcore: registered new driver usbfs
usbcore: registered new driver hub
PCI: System does not support PCI
PCI: System does not support PCI
NetLabel: Initializing
NetLabel:  domain <span style="color: #7a0874; font-weight: bold;">hash</span> <span style="color: #c20cb9; font-weight: bold;">size</span> = <span style="color: #000000;">128</span>
NetLabel:  protocols = UNLABELED CIPSOv4
NetLabel:  unlabeled traffic allowed by default
NET: Registered protocol family <span style="color: #000000;">2</span>
IP route cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">32768</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">5</span>, <span style="color: #000000;">131072</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP established <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">8</span>, <span style="color: #000000;">1048576</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP <span style="color: #7a0874; font-weight: bold;">bind</span> <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">65536</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">7</span>, <span style="color: #000000;">524288</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP: Hash tables configured <span style="color: #7a0874; font-weight: bold;">&#40;</span>established <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">bind</span> <span style="color: #000000;">65536</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP reno registered
audit: initializing netlink socket <span style="color: #7a0874; font-weight: bold;">&#40;</span>disabled<span style="color: #7a0874; font-weight: bold;">&#41;</span>
<span style="color: #007800;">type</span>=<span style="color: #000000;">2000</span> audit<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1338820837.118</span>:<span style="color: #000000;">1</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: initialized
highmem bounce pool <span style="color: #c20cb9; font-weight: bold;">size</span>: <span style="color: #000000;">64</span> pages
VFS: Disk quotas dquot_6.5.1
Dquot-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">1024</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order <span style="color: #000000;"></span>, <span style="color: #000000;">4096</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Initializing Cryptographic API
alg: No <span style="color: #7a0874; font-weight: bold;">test</span> <span style="color: #000000; font-weight: bold;">for</span> crc32c <span style="color: #7a0874; font-weight: bold;">&#40;</span>crc32c-generic<span style="color: #7a0874; font-weight: bold;">&#41;</span>
ksign: Installing public key data
Loading keyring
- Added public key E4B7FFDB66F0F649
- User ID: CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>Kernel Module GPG key<span style="color: #7a0874; font-weight: bold;">&#41;</span>
io scheduler noop registered
io scheduler anticipatory registered
io scheduler deadline registered
io scheduler cfq registered <span style="color: #7a0874; font-weight: bold;">&#40;</span>default<span style="color: #7a0874; font-weight: bold;">&#41;</span>
pci_hotplug: PCI Hot Plug PCI Core version: <span style="color: #000000;">0.5</span>
rtc: IRQ <span style="color: #000000;">8</span> is not free.
Non-volatile memory driver v1.2
Linux agpgart interface v0.101 <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> Dave Jones
brd: module loaded
Xen virtual console successfully installed <span style="color: #c20cb9; font-weight: bold;">as</span> xvc0
Linux version 2.6.18-<span style="color: #000000;">308</span>.el5xen <span style="color: #7a0874; font-weight: bold;">&#40;</span>mockbuild<span style="color: #000000; font-weight: bold;">@</span>builder10.centos.org<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #c20cb9; font-weight: bold;">gcc</span> version 4.1.2 <span style="color: #000000;">20080704</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Red Hat 4.1.2-<span style="color: #000000;">52</span><span style="color: #7a0874; font-weight: bold;">&#41;</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #666666; font-style: italic;">#1 SMP Tue Feb 21 21:26:03 EST 2012</span>
BIOS-provided physical RAM map:
 Xen: 0000000000000000 - 0000000040800000 <span style="color: #7a0874; font-weight: bold;">&#40;</span>usable<span style="color: #7a0874; font-weight: bold;">&#41;</span>
304MB HIGHMEM available.
727MB LOWMEM available.
NX <span style="color: #7a0874; font-weight: bold;">&#40;</span>Execute Disable<span style="color: #7a0874; font-weight: bold;">&#41;</span> protection: active
ACPI <span style="color: #000000; font-weight: bold;">in</span> unprivileged domain disabled
Built <span style="color: #000000;">1</span> zonelists.  Total pages: <span style="color: #000000;">264192</span>
Kernel <span style="color: #7a0874; font-weight: bold;">command</span> line:  ro <span style="color: #007800;">root</span>=<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 <span style="color: #007800;">console</span>=xvc0
Enabling fast FPU save and restore... done.
Enabling unmasked SIMD FPU exception support... done.
Initializing CPU<span style="color: #666666; font-style: italic;">#0</span>
CPU <span style="color: #000000;"></span> irqstacks, <span style="color: #007800;">hard</span>=c0784000 <span style="color: #007800;">soft</span>=c0764000
PID <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">4096</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">12</span>, <span style="color: #000000;">16384</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Xen reported: <span style="color: #000000;">2400.084</span> MHz processor.
Console: colour dummy device 80x25
Dentry cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">7</span>, <span style="color: #000000;">524288</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Inode-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">65536</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">6</span>, <span style="color: #000000;">262144</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Software IO TLB disabled
vmalloc area: ee000000-f4ffe000, maxmem 2d7fe000
Memory: 1024372k<span style="color: #000000; font-weight: bold;">/</span>1056768k available <span style="color: #7a0874; font-weight: bold;">&#40;</span>2212k kernel code, 23552k reserved, 1038k data, 184k init, 311304k highmem<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Checking <span style="color: #000000; font-weight: bold;">if</span> this processor honours the WP bit even <span style="color: #000000; font-weight: bold;">in</span> supervisor mode... Ok.
Calibrating delay using timer specific routine.. <span style="color: #000000;">6002.86</span> BogoMIPS <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #007800;">lpj</span>=<span style="color: #000000;">12005729</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
Security Framework v1.0.0 initialized
SELinux:  Initializing.
selinux_register_security:  Registering secondary module capability
Capability LSM initialized <span style="color: #c20cb9; font-weight: bold;">as</span> secondary
Mount-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">512</span>
CPU: L1 I cache: 32K, L1 D cache: 32K
CPU: L2 cache: 256K
CPU: L3 cache: 8192K
Checking <span style="color: #ff0000;">'hlt'</span> instruction... OK.
SMP alternatives: switching to UP code
Freeing SMP alternatives: 14k freed
Brought up <span style="color: #000000;">1</span> CPUs
checking <span style="color: #000000; font-weight: bold;">if</span> image is initramfs... it is
Freeing initrd memory: 7304k freed
Grant table initialized
NET: Registered protocol family <span style="color: #000000;">16</span>
Brought up <span style="color: #000000;">1</span> CPUs
PCI: setting up Xen PCI frontend stub
ACPI: Interpreter disabled.
Linux Plug and Play Support v0.97 <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> Adam Belay
pnp: PnP ACPI: disabled
xen_mem: Initialising balloon driver.
usbcore: registered new driver usbfs
usbcore: registered new driver hub
PCI: System does not support PCI
PCI: System does not support PCI
NetLabel: Initializing
NetLabel:  domain <span style="color: #7a0874; font-weight: bold;">hash</span> <span style="color: #c20cb9; font-weight: bold;">size</span> = <span style="color: #000000;">128</span>
NetLabel:  protocols = UNLABELED CIPSOv4
NetLabel:  unlabeled traffic allowed by default
NET: Registered protocol family <span style="color: #000000;">2</span>
IP route cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">32768</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">5</span>, <span style="color: #000000;">131072</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP established <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">8</span>, <span style="color: #000000;">1048576</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP <span style="color: #7a0874; font-weight: bold;">bind</span> <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">65536</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order: <span style="color: #000000;">7</span>, <span style="color: #000000;">524288</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP: Hash tables configured <span style="color: #7a0874; font-weight: bold;">&#40;</span>established <span style="color: #000000;">131072</span> <span style="color: #7a0874; font-weight: bold;">bind</span> <span style="color: #000000;">65536</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
TCP reno registered
audit: initializing netlink socket <span style="color: #7a0874; font-weight: bold;">&#40;</span>disabled<span style="color: #7a0874; font-weight: bold;">&#41;</span>
<span style="color: #007800;">type</span>=<span style="color: #000000;">2000</span> audit<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1338820837.118</span>:<span style="color: #000000;">1</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: initialized
highmem bounce pool <span style="color: #c20cb9; font-weight: bold;">size</span>: <span style="color: #000000;">64</span> pages
VFS: Disk quotas dquot_6.5.1
Dquot-cache <span style="color: #7a0874; font-weight: bold;">hash</span> table entries: <span style="color: #000000;">1024</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>order <span style="color: #000000;"></span>, <span style="color: #000000;">4096</span> bytes<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Initializing Cryptographic API
alg: No <span style="color: #7a0874; font-weight: bold;">test</span> <span style="color: #000000; font-weight: bold;">for</span> crc32c <span style="color: #7a0874; font-weight: bold;">&#40;</span>crc32c-generic<span style="color: #7a0874; font-weight: bold;">&#41;</span>
ksign: Installing public key data
Loading keyring
- Added public key E4B7FFDB66F0F649
- User ID: CentOS <span style="color: #7a0874; font-weight: bold;">&#40;</span>Kernel Module GPG key<span style="color: #7a0874; font-weight: bold;">&#41;</span>
io scheduler noop registered
io scheduler anticipatory registered
io scheduler deadline registered
io scheduler cfq registered <span style="color: #7a0874; font-weight: bold;">&#40;</span>default<span style="color: #7a0874; font-weight: bold;">&#41;</span>
pci_hotplug: PCI Hot Plug PCI Core version: <span style="color: #000000;">0.5</span>
rtc: IRQ <span style="color: #000000;">8</span> is not free.
Non-volatile memory driver v1.2
Linux agpgart interface v0.101 <span style="color: #7a0874; font-weight: bold;">&#40;</span>c<span style="color: #7a0874; font-weight: bold;">&#41;</span> Dave Jones
brd: module loaded
Xen virtual console successfully installed <span style="color: #c20cb9; font-weight: bold;">as</span> xvc0
Event-channel device installed.
Uniform Multi-Platform E-IDE driver Revision: 7.00alpha2
ide: Assuming 50MHz system bus speed <span style="color: #000000; font-weight: bold;">for</span> PIO modes; override with <span style="color: #007800;">idebus</span>=xx
ide-floppy driver <span style="color: #000000;">0.99</span>.newide
usbcore: registered new driver hiddev
usbcore: registered new driver usbhid
drivers<span style="color: #000000; font-weight: bold;">/</span>usb<span style="color: #000000; font-weight: bold;">/</span>input<span style="color: #000000; font-weight: bold;">/</span>hid-core.c: v2.6:USB HID core driver
PNP: No PS<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">2</span> controller found. Probing ports directly.
i8042.c: No controller found.
mice: PS<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">2</span> mouse device common <span style="color: #000000; font-weight: bold;">for</span> all mice
md: md driver 0.90.3 <span style="color: #007800;">MAX_MD_DEVS</span>=<span style="color: #000000;">256</span>, <span style="color: #007800;">MD_SB_DISKS</span>=<span style="color: #000000;">27</span>
md: bitmap version <span style="color: #000000;">4.39</span>
TCP bic registered
Initializing IPsec netlink socket
NET: Registered protocol family <span style="color: #000000;">1</span>
NET: Registered protocol family <span style="color: #000000;">17</span>
Using IPI No-Shortcut mode
XENBUS: Device with no driver: device<span style="color: #000000; font-weight: bold;">/</span>vbd<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">51712</span>
XENBUS: Device with no driver: device<span style="color: #000000; font-weight: bold;">/</span>vif<span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;"></span>
Initalizing network drop monitor service
Freeing unused kernel memory: 184k freed
Write protecting the kernel read-only data: 401k
Red Hat nash version 5.1.19.6 starting
Mounting proc filesystem
Mounting sysfs filesystem
Creating <span style="color: #000000; font-weight: bold;">/</span>dev
Creating initial device nodes
Setting up hotplug.
Creating block device nodes.
Loading ehci-hcd.ko module
Loading ohci-hcd.ko module
Loading uhci-hcd.ko module
USB Universal Host Controller Interface driver v3.0
Loading jbd.ko module
Loading ext3.ko module
Loading xenblk.ko module
Registering block device major <span style="color: #000000;">202</span>
 xvda: xvda1 xvda2
Loading dm-mod.ko module
device-mapper: uevent: version 1.0.3
device-mapper: ioctl: 4.11.6-ioctl <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">2011</span>-02-<span style="color: #000000;">18</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> initialised: dm-devel<span style="color: #000000; font-weight: bold;">@</span>redhat.com
Loading dm-log.ko module
Loading dm-mirror.ko module
Loading dm-zero.ko module
Loading dm-snapshot.ko module
Loading dm-mem-cache.ko module
Loading dm-region_hash.ko module
Loading dm-message.ko module
Loading dm-raid45.ko module
device-mapper: dm-raid45: initialized v0.2594l
Scanning and configuring dmraid supported devices
Scanning logical volumes
  Reading all physical volumes.  This may take a while...
  Found volume group <span style="color: #ff0000;">"VolGroup00"</span> using metadata <span style="color: #7a0874; font-weight: bold;">type</span> lvm2
Activating logical volumes
  <span style="color: #000000;">2</span> logical volume<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000; font-weight: bold;">in</span> volume group <span style="color: #ff0000;">"VolGroup00"</span> now active
Creating root device.
Mounting root filesystem.
kjournald starting.  Commit interval <span style="color: #000000;">5</span> seconds
EXT3-fs: mounted filesystem with ordered data mode.
Setting up other filesystems.
Setting up new root fs
no fstab.sys, mounting internal defaults
Switching to new root and running init.
unmounting old <span style="color: #000000; font-weight: bold;">/</span>dev
unmounting old <span style="color: #000000; font-weight: bold;">/</span>proc
unmounting old <span style="color: #000000; font-weight: bold;">/</span>sys
<span style="color: #007800;">type</span>=<span style="color: #000000;">1404</span> audit<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1338820841.279</span>:<span style="color: #000000;">2</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: <span style="color: #007800;">enforcing</span>=<span style="color: #000000;">1</span> <span style="color: #007800;">old_enforcing</span>=<span style="color: #000000;"></span> <span style="color: #007800;">auid</span>=<span style="color: #000000;">4294967295</span> <span style="color: #007800;">ses</span>=<span style="color: #000000;">4294967295</span>
<span style="color: #007800;">type</span>=<span style="color: #000000;">1403</span> audit<span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1338820841.487</span>:<span style="color: #000000;">3</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>: policy loaded <span style="color: #007800;">auid</span>=<span style="color: #000000;">4294967295</span> <span style="color: #007800;">ses</span>=<span style="color: #000000;">4294967295</span>
INIT: version <span style="color: #000000;">2.86</span> booting
		Welcome to  CentOS release <span style="color: #000000;">5.8</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Final<span style="color: #7a0874; font-weight: bold;">&#41;</span>
		Press <span style="color: #ff0000;">'I'</span> to enter interactive startup.
Configurando relógio  <span style="color: #7a0874; font-weight: bold;">&#40;</span>utc<span style="color: #7a0874; font-weight: bold;">&#41;</span>: Seg Jun  <span style="color: #000000;">4</span> <span style="color: #000000;">11</span>:<span style="color: #000000;">40</span>:<span style="color: #000000;">43</span> BRT <span style="color: #000000;">2012</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando udev: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Carregando o mapa <span style="color: #000000; font-weight: bold;">do</span> teclado padrão <span style="color: #7a0874; font-weight: bold;">&#40;</span>us<span style="color: #7a0874; font-weight: bold;">&#41;</span>: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Definindo o nome da máquina bapi:  <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Configurando a Gestão de Volumes Lógicos <span style="color: #7a0874; font-weight: bold;">&#40;</span>LVM<span style="color: #7a0874; font-weight: bold;">&#41;</span>:   <span style="color: #000000;">2</span> logical volume<span style="color: #7a0874; font-weight: bold;">&#40;</span>s<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #000000; font-weight: bold;">in</span> volume group <span style="color: #ff0000;">"VolGroup00"</span> now active <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Verificando sistemas de arquivo
Checking all <span style="color: #c20cb9; font-weight: bold;">file</span> systems.
<span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000; font-weight: bold;">/</span>sbin<span style="color: #000000; font-weight: bold;">/</span>fsck.ext3 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #660033;">--</span> <span style="color: #000000; font-weight: bold;">/</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> fsck.ext3 <span style="color: #660033;">-a</span> <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00 
<span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>VolGroup00<span style="color: #000000; font-weight: bold;">/</span>LogVol00: clean, <span style="color: #000000;">50691</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">10190848</span> files, <span style="color: #000000;">665212</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">10182656</span> blocks
<span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000; font-weight: bold;">/</span>sbin<span style="color: #000000; font-weight: bold;">/</span>fsck.ext3 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">1</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #660033;">--</span> <span style="color: #000000; font-weight: bold;">/</span>boot<span style="color: #7a0874; font-weight: bold;">&#93;</span> fsck.ext3 <span style="color: #660033;">-a</span> <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>xvda1 
<span style="color: #000000; font-weight: bold;">/</span>boot: clean, <span style="color: #000000;">37</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">26104</span> files, <span style="color: #000000;">17108</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #000000;">104388</span> blocks <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Remontando o sistema de arquivo root no modo leitura-escrita:  <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Montando sistemas de arquivo locais:  <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Ativando quotas dos sistemas de arquivos locais: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Ativando a memória virtual no <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>fstab:  <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
INIT: Entering runlevel: <span style="color: #000000;">3</span>
Iniciando startup não-interativo
Aplicando atualização de Microcódigo de CPU Intel: <span style="color: #7a0874; font-weight: bold;">&#91;</span>FALHOU<span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando leitura <span style="color: #c20cb9; font-weight: bold;">pr</span>évia <span style="color: #7a0874; font-weight: bold;">&#40;</span>readahead<span style="color: #7a0874; font-weight: bold;">&#41;</span> em segundo plano: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Procurando mudanç<span style="color: #c20cb9; font-weight: bold;">as</span> no hardware <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
iSCSI daemon: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Aplicando regras ao firewall iptables: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Carregando módulos iptables adicionais: ip_conntrack_netbios_ns <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando mcstransd: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando a interface <span style="color: #ff0000;">'loopback'</span>:  <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando interface eth0:   <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando auditd: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando restorecond: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando o registrador <span style="color: #000000; font-weight: bold;">do</span> sistema: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando o servidor de registros <span style="color: #000000; font-weight: bold;">do</span> kernel: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando irqbalance: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
iscsid <span style="color: #7a0874; font-weight: bold;">&#40;</span>pid  <span style="color: #000000;">867</span><span style="color: #7a0874; font-weight: bold;">&#41;</span> está rodando...
Estabelecendo alvos iSCSI: iscsiadm: No records found <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando portmap: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando o NFS statd: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
Iniciando smartd: <span style="color: #7a0874; font-weight: bold;">&#91;</span>  OK  <span style="color: #7a0874; font-weight: bold;">&#93;</span>
&nbsp;
CentOS release <span style="color: #000000;">5.8</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>Final<span style="color: #7a0874; font-weight: bold;">&#41;</span>
Kernel 2.6.18-<span style="color: #000000;">308</span>.el5xen on an i686
&nbsp;
bapi <span style="color: #c20cb9; font-weight: bold;">login</span>:</pre>
        </td>
      </tr>
    </table>
  </div>
  
  <p>
    <span id="saidaqui"></span><br /> Agora é com você, faça login e divirta-se.
  </p>
  
  <p>
    <del datetime="2012-06-15T16:19:42+00:00">Eu vou transformar esta instalação <strong>CentOS</strong> um proxy reverso com Nginx para utilizar uma aplicação Tornado, quando terminar faço um pingback para este post.</del>
  </p>
  
  <p>
    Parte da instalação já está pronta, acesse o post abaixo para ver como transformar este <strong>CentOS</strong> em um servidor de aplicação <strong>Tornado</strong>.<br /> <a href="http://www.gabrielfernandes.org/2012/06/15/configurar-o-centos-5-para-suporte-ao-tornado-no-python-26/" title="Clique e continue a implantação deste servidor"><br /> Configurar o CentOS 5 para suporte ao Tornado no Python 2.6</a>
  </p>
  
  <p>
    Valeu.
  </p>