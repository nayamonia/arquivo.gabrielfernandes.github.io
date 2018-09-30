---
id: 960
title: Como configurar servidor NFS e compartilhar arquivos no Centos 5
date: 2012-04-30T16:56:32+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=960
permalink: /2012/04/30/como-configurar-um-servidor-nfs-e-compartilhar-arquivos-no-centos/
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
image: /wp-content/uploads/2012/04/need-for-speed-most-wanted-screenshot-3.jpeg
categories:
  - linux
  - rede
  - segurança
  - shell
tags:
  - centos
  - compartilhar
  - nfs
  - servidor
---
Compartilhar arquivos por NFS é tarefa recorrente no meu ambiente de trabalho. É o tipo de coisa simples e bastante útil que todo administrador Linux deve poder contar, veja como configurar um servidor NFS simples e totalmente funcional, segue:

Certifique-se que os requisitos de software estejam completados, instalando o _**nfs**_-utils e o _portmap_:

<div class="wp_codebox">
  <table>
    <tr id="p960160">
      <td class="code" id="p960code160">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> nfs-utils portmap</pre>
      </td>
    </tr>
  </table>
</div>

<!--more [CONTINUAR LENDO]-->


  
Execute os comandos abaixo, para configurar a inicialização automática destes serviços, pois caso ainda não estejam configurados desta forma, após o comando, eles estarão:

<div class="wp_codebox">
  <table>
    <tr id="p960161">
      <td class="code" id="p960code161">
        <pre class="bash" style="font-family:monospace;">chkconfig <span style="color: #660033;">--level</span> <span style="color: #000000;">2345</span> nfs on</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p960162">
      <td class="code" id="p960code162">
        <pre class="bash" style="font-family:monospace;">chkconfig <span style="color: #660033;">--level</span> <span style="color: #000000;">2345</span> nfslock on</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p960163">
      <td class="code" id="p960code163">
        <pre class="bash" style="font-family:monospace;">chkconfig <span style="color: #660033;">--level</span> <span style="color: #000000;">2345</span> portmap on</pre>
      </td>
    </tr>
  </table>
</div>

Inclua no arquivo _/etc/exports_ os diretórios/pastas que serão compartilhados, no exemplo a seguir vamos compartilhar um diretório no raiz de nome _/path_comum_:

Use o _vi_ &#8211; ou editor de sua preferência &#8211; para criar e editar o arquivo _exports_:

<div class="wp_codebox">
  <table>
    <tr id="p960164">
      <td class="code" id="p960code164">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>exports</pre>
      </td>
    </tr>
  </table>
</div>

Inclua a linha _/caminho/completo IP\_do\_Servidor(opções)_, para nosso exemplo o arquivo ficaria assim:

<div class="wp_codebox">
  <table>
    <tr id="p960165">
      <td class="code" id="p960code165">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">/</span>path_comum 192.168.254.221<span style="color: #7a0874; font-weight: bold;">&#40;</span>rw,<span style="color: #c20cb9; font-weight: bold;">sync</span><span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
      </td>
    </tr>
  </table>
</div>

Precisamos garantir que algumas portas estejam abertas no firewall, para tanto vamos definir na configuração do NFS as portas padrões e depois ajustar algumas configurações do iptables.

Comece editando o _/etc/sysconfig/nfs_. Novamente sugiro usar o _vi_, mas isto é uma decisão sua:

<div class="wp_codebox">
  <table>
    <tr id="p960166">
      <td class="code" id="p960code166">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>sysconfig<span style="color: #000000; font-weight: bold;">/</span>nfs</pre>
      </td>
    </tr>
  </table>
</div>

Nele devemos procurar pelas linhas descritas abaixo e retirar o carácter de comentário do início da linha &#8211; caso houver.
  
Importante ressaltar que estas linhas NÃO estarão em sequência, você deve navegar nas linhas do arquivo para encontrar cada uma delas.

<div class="wp_codebox">
  <table>
    <tr id="p960167">
      <td class="code" id="p960code167">
        <pre class="bash" style="font-family:monospace;"><span style="color: #007800;">LOCKD_TCPPORT</span>=<span style="color: #000000;">32803</span>
<span style="color: #007800;">LOCKD_UDPPORT</span>=<span style="color: #000000;">32769</span>
<span style="color: #007800;">MOUNTD_PORT</span>=<span style="color: #000000;">892</span>
<span style="color: #007800;">STATD_PORT</span>=<span style="color: #000000;">662</span></pre>
      </td>
    </tr>
  </table>
</div>

Agora vamos editar o _/etc/sysconfig/iptables_, muito cuidado neste item para não destruir nenhuma regra existente no firewall.

<div class="wp_codebox">
  <table>
    <tr id="p960168">
      <td class="code" id="p960code168">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>sysconfig<span style="color: #000000; font-weight: bold;">/</span>iptables</pre>
      </td>
    </tr>
  </table>
</div>

Se você não alterou o valor de nenhuma das portas no arquivo _/etc/sysconfig/nfs_, suas novas regras de firewall para o iptables serão como a seguir.
  
Note que é importante que estas regras sejam inseridas no arquivo, ou seja, o conteúdo existente no arquivo deve permanecer e tenha certeza de incluir todas as linhas antes da linha **COMMIT** e de qualquer outra regra **REJECT**:

<div class="wp_codebox">
  <table>
    <tr id="p960169">
      <td class="code" id="p960code169">
        <pre class="bash" style="font-family:monospace;"><span style="color: #666666; font-style: italic;"># NFS</span>
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">111</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">111</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">662</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">662</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">892</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">892</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">2049</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">2049</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">32803</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">32803</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> tcp <span style="color: #660033;">-p</span> tcp <span style="color: #660033;">--dport</span> <span style="color: #000000;">32769</span> <span style="color: #660033;">-j</span> ACCEPT
<span style="color: #660033;">-A</span> RH-Firewall-<span style="color: #000000;">1</span>-INPUT <span style="color: #660033;">-m</span> state <span style="color: #660033;">--state</span> NEW <span style="color: #660033;">-m</span> udp <span style="color: #660033;">-p</span> udp <span style="color: #660033;">--dport</span> <span style="color: #000000;">32769</span> <span style="color: #660033;">-j</span> ACCEPT</pre>
      </td>
    </tr>
  </table>
</div>

Terminadas as regras de firewall &#8211; parte mais crítica e fundamental para o funcionamento &#8211; vamos reiniciar/iniciar todos os serviços necessários:

<div class="wp_codebox">
  <table>
    <tr id="p960170">
      <td class="code" id="p960code170">
        <pre class="bash" style="font-family:monospace;">service iptables restart</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p960171">
      <td class="code" id="p960code171">
        <pre class="bash" style="font-family:monospace;">service nfs restart</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p960172">
      <td class="code" id="p960code172">
        <pre class="bash" style="font-family:monospace;">service nfslock restart</pre>
      </td>
    </tr>
  </table>
</div>

> Se estiver com dificuldades com o firewall, você pode optar por desativa-lo temporariamente, para o serviço _iptables_

Se tudo correu bem, temos um servidor NFS simplificado e totalmente funcional.

Para mapear uma unidade NFS como está que fizemos, use:

<div class="wp_codebox">
  <table>
    <tr id="p960173">
      <td class="code" id="p960code173">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">mount</span> <span style="color: #660033;">-t</span> nfs <span style="color: #660033;">-orw</span> 192.168.19.202:<span style="color: #000000; font-weight: bold;">/</span>path_comum path_comum19</pre>
      </td>
    </tr>
  </table>
</div>

Ou se preferir que a montagem seja persistente, insira a linha abaixo no arquivo _/etc/fstab_.
  
Esta linha monta o compartilhamento **NFS** _192.168.19.202:/path_comum_ na pasta _/Zanthus/Zeus/path_comum19_ do computador local.

<div class="wp_codebox">
  <table>
    <tr id="p960174">
      <td class="code" id="p960code174">
        <pre class="bash" style="font-family:monospace;">192.168.19.202:<span style="color: #000000; font-weight: bold;">/</span>path_comum <span style="color: #000000; font-weight: bold;">/</span>Zanthus<span style="color: #000000; font-weight: bold;">/</span>Zeus<span style="color: #000000; font-weight: bold;">/</span>path_comum19 nfs hard,intr <span style="color: #000000;"></span> <span style="color: #000000;"></span></pre>
      </td>
    </tr>
  </table>
</div>

É isto ai, qualquer adversidade, pergunte.