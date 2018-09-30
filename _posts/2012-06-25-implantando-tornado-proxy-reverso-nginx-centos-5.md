---
id: 1416
title: Implantando Tornado com proxy reverso Nginx no CentOS 5
date: 2012-06-25T16:24:54+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1416
permalink: /2012/06/25/implantando-tornado-proxy-reverso-nginx-centos-5/
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
image: /wp-content/uploads/2012/06/nginx1-100x100.png
categories:
  - automação
  - desenvolvimento
  - linux
  - programação
  - segurança
tags:
  - centos
  - nginx
  - proxy reverso
  - python
  - tornado
---
Este é o penúltimo da sequência de posts sobre uma implantação **Tornado com proxy reverso Nginx no <a href="http://www.gabrielfernandes.org/tag/centos/" target="_blank">CentOS</a> 5**. O objetivo é ter um servidor que receba as conexões por uma única porta e distribua para as várias instâncias de **Tornado**, cada uma em porta diferente, no fim teremos algo como a figura abaixo:<figure id="attachment_1537" style="width: 602px" class="wp-caption aligncenter">

[<img src="https://i0.wp.com/www.gabrielfernandes.org/wp-content/uploads/2012/06/nginx1.png?resize=602%2C141" alt="" title="Tornado atrás de um proxy reverso" width="602" height="141" class="size-full wp-image-1537" srcset="https://i0.wp.com/www.gabrielfernandes.org/wp-content/uploads/2012/06/nginx1.png?w=602 602w, https://i0.wp.com/www.gabrielfernandes.org/wp-content/uploads/2012/06/nginx1.png?resize=300%2C70 300w" sizes="(max-width: 602px) 100vw, 602px" data-recalc-dims="1" />](https://i0.wp.com/www.gabrielfernandes.org/wp-content/uploads/2012/06/nginx1.png)<figcaption class="wp-caption-text">Tornado atrás de um proxy reverso</figcaption></figure> 

<!--more [CONTINUAR LENDO]-->

> O **Nginx** por padrão distribui as requisições no estilo <a href="http://pt.wikipedia.org/wiki/Round-robin_%28algoritmo%29" title="Eu vou te levar para o Wikipédia, mas não esqueça de retornar." target="_blank">round-robin</a>, tu podes alterar isto estudando um pouco sobre a opção <a href="http://wiki.nginx.org/HttpUpstreamModule" title="Os bons sempre voltam, de uma lida e volte pra cá!" target="_blank">HTTPUpstreamModule</a> do **Nginx**. 

Para seguir as instruções e obter sucesso nos procedimentos tu precisas de um ambiente **<a href="http://www.gabrielfernandes.org/tag/centos/" target="_blank">CentOS</a> 5** com **Python** e **Tornado** instalados, se tu não tem isto pronto, leia estes outros posts antes de prosseguir: 

  * <a href="http://www.gabrielfernandes.org/2012/06/04/como-criar-e-instalar-o-centos-em-uma-maquina-virtual-no-servidor-xen-centos-5/" title="Clique e veja como criar uma VM e instalar o CentOS em um Servidor Xen" target="_blank">Como criar uma máquina virtual no servidor <strong>Xen Centos 5</strong> e instalar o <strong><a href="http://www.gabrielfernandes.org/tag/centos/" target="_blank">CentOS</a></strong> pela rede</a> 

  * <a href="http://www.gabrielfernandes.org/2012/06/15/configurar-o-centos-5-para-suporte-ao-tornado-no-python-26/" title="Clique aqui e veja como instalei o Python 2.6 e o Tornado" target="_blank">Configurar o <strong>CentOS</strong> 5 para suporte ao <strong>Tornado</strong> no <strong>Python 2.6</strong></a> 

  * <a href="http://www.gabrielfernandes.org/2012/06/16/tornado-e-cxoracle-no-python-26-em-centos-5-com-oracle-instant-client/" title="Isto é opcional. Veja aqui como fazer o Python ter suporte a Oracle" target="_blank">Tornado e cx_Oracle no Python 2.6 em CentOS 5 com Oracle Instant Client</a> 

Instalei o servidor web **Nginx** pelo yum, para que isto fosse possível tive que adicionar o repositório do **Nginx** nas configurações do yum.

Assim criei um arquivo em _/etc/yum.repos.d/_, como o exemplo abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p1416242">
      <td class="code" id="p1416code242">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>yum.repos.d<span style="color: #000000; font-weight: bold;">/</span>nginx.repo</pre>
      </td>
    </tr>
  </table>
</div>

Adicionei ao arquivo _nginx.repo_ recém criado as linhas que seguem:

<div class="wp_codebox">
  <table>
    <tr id="p1416243">
      <td class="code" id="p1416code243">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">&#91;</span>nginx<span style="color: #7a0874; font-weight: bold;">&#93;</span>
<span style="color: #007800;">name</span>=nginx repo
<span style="color: #007800;">baseurl</span>=http:<span style="color: #000000; font-weight: bold;">//</span>nginx.org<span style="color: #000000; font-weight: bold;">/</span>packages<span style="color: #000000; font-weight: bold;">/</span>centos<span style="color: #000000; font-weight: bold;">/</span><span style="color: #007800;">$releasever</span><span style="color: #000000; font-weight: bold;">/</span><span style="color: #007800;">$basearch</span><span style="color: #000000; font-weight: bold;">/</span>
<span style="color: #007800;">gpgcheck</span>=<span style="color: #000000;"></span>
<span style="color: #007800;">enabled</span>=<span style="color: #000000;">1</span></pre>
      </td>
    </tr>
  </table>
</div>

Pronto, mandei instalar assim:

<div class="wp_codebox">
  <table>
    <tr id="p1416244">
      <td class="code" id="p1416code244">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> nginx</pre>
      </td>
    </tr>
  </table>
</div>

Após terminar fiz um teste bobo como este:

<div class="wp_codebox">
  <table>
    <tr id="p1416245">
      <td class="code" id="p1416code245">
        <pre class="bash" style="font-family:monospace;">service nginx start</pre>
      </td>
    </tr>
  </table>
</div>

E tudo correu bem, sem erros.

Fui para a configuração, mudei pouca coisa do arquivo padrão da instalação, apenas o parâmetro _worker_processes_ que define o número de processos de trabalho, o valor ideal deste parâmetro depende de vários fatores, inclusive, mas não limitado, ao número de cores do processador. Se você não souber como quantificar, colocar a quantidade de cores do processador é um bom começo.

Adicionei também o parâmetro _use epoll_ em _events_ para &#8220;_forçar_&#8221; o uso deste modelo de notificação de eventos.

Então abri e editei o arquivo das configurações do **Nginx** com o comando que segue:

<div class="wp_codebox">
  <table>
    <tr id="p1416246">
      <td class="code" id="p1416code246">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>nginx<span style="color: #000000; font-weight: bold;">/</span>nginx.conf</pre>
      </td>
    </tr>
  </table>
</div>

Meu arquivo ficou assim:

<div class="wp_codebox">
  <table>
    <tr id="p1416247">
      <td class="code" id="p1416code247">
        <pre class="python" style="font-family:monospace;"><span style="color: #dc143c;">user</span>  nginx<span style="color: #66cc66;">;</span>
worker_processes  <span style="color: #ff4500;">5</span><span style="color: #66cc66;">;</span>
&nbsp;
error_log  /var/log/nginx/error.<span style="color: black;">log</span> warn<span style="color: #66cc66;">;</span>
pid        /var/run/nginx.<span style="color: black;">pid</span><span style="color: #66cc66;">;</span>
&nbsp;
events <span style="color: black;">&#123;</span>
    worker_connections  <span style="color: #ff4500;">1024</span><span style="color: #66cc66;">;</span>
    use epoll<span style="color: #66cc66;">;</span>
<span style="color: black;">&#125;</span>
&nbsp;
http <span style="color: black;">&#123;</span>
    include       /etc/nginx/mime.<span style="color: #dc143c;">types</span><span style="color: #66cc66;">;</span>
    default_type  application/octet-stream<span style="color: #66cc66;">;</span>
    log_format  main  <span style="color: #483d8b;">'$remote_addr - $remote_user [$time_local] "$request" '</span>
                      <span style="color: #483d8b;">'$status $body_bytes_sent "$http_referer" '</span>
                      <span style="color: #483d8b;">'"$http_user_agent" "$http_x_forwarded_for"'</span><span style="color: #66cc66;">;</span>
    access_log  /var/log/nginx/access.<span style="color: black;">log</span>  main<span style="color: #66cc66;">;</span>
    sendfile        on<span style="color: #66cc66;">;</span>
    keepalive_timeout  <span style="color: #ff4500;">65</span><span style="color: #66cc66;">;</span>
    include /etc/nginx/conf.<span style="color: black;">d</span>/<span style="color: #66cc66;">*</span>.<span style="color: black;">conf</span><span style="color: #66cc66;">;</span>
<span style="color: black;">&#125;</span></pre>
      </td>
    </tr>
  </table>
</div>

Belezas, este ai acima é o arquivo das configurações &#8220;comuns&#8221; do **Nginx**, abaixo criei um arquivo para as configurações do **proxy reverso** da minha implantação **Tornado**.

Inicialmente:

<div class="wp_codebox">
  <table>
    <tr id="p1416248">
      <td class="code" id="p1416code248">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">vi</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>nginx<span style="color: #000000; font-weight: bold;">/</span>conf.d<span style="color: #000000; font-weight: bold;">/</span>bapi.conf</pre>
      </td>
    </tr>
  </table>
</div>

> Tu podes escolher outro nome de arquivo, desde que terminado com &#8220;.conf&#8221;. O _bapi.conf_ daqui é em homenagem ao nome de batismo do meu aplicativo **Tornado**. 

Legal, mas aqui não vou entrar em detalhe sobre cada um dos parâmetros &#8211; ficaria muito extenso nosso papo e este não é o meu objetivo -, quero apenas dizer que adicionaremos no parâmetro de grupo de servidores _upstream_ as instancias **Tornado** na qual este **Nginx** fará proxy reverso e no _proxy_pass_ a referencia para o grupo de servidores _upstream_.

> Para saber mais sobre cada um destes parâmetros, acesse a wiki oficial do Nginx pelo link: <a href="http://wiki.nginx.org/DirectiveIndex" target="_blank">http://wiki.nginx.org/DirectiveIndex</a>.

Meu arquivo ficou assim:

<div class="wp_codebox">
  <table>
    <tr id="p1416249">
      <td class="code" id="p1416code249">
        <pre class="python" style="font-family:monospace;">&nbsp;
proxy_next_upstream error<span style="color: #66cc66;">;</span>
&nbsp;
upstream bapis <span style="color: black;">&#123;</span>
    server 127.0.0.1:<span style="color: #ff4500;">8080</span><span style="color: #66cc66;">;</span>
    server 127.0.0.1:<span style="color: #ff4500;">8081</span><span style="color: #66cc66;">;</span>
<span style="color: black;">&#125;</span>
&nbsp;
server <span style="color: black;">&#123;</span>
    listen       <span style="color: #ff4500;">8888</span><span style="color: #66cc66;">;</span>
    server_name  bapih.<span style="color: black;">bis</span>.<span style="color: black;">com</span>.<span style="color: black;">br</span><span style="color: #66cc66;">;</span>
&nbsp;
    location /static/ <span style="color: black;">&#123;</span>
        root /var/www/static<span style="color: #66cc66;">;</span>
        <span style="color: #ff7700;font-weight:bold;">if</span> <span style="color: black;">&#40;</span>$query_string<span style="color: black;">&#41;</span> <span style="color: black;">&#123;</span>
            expires <span style="color: #008000;">max</span><span style="color: #66cc66;">;</span>
        <span style="color: black;">&#125;</span>
    <span style="color: black;">&#125;</span>
&nbsp;
    location / <span style="color: black;">&#123;</span>
        proxy_pass_header Server<span style="color: #66cc66;">;</span>
        proxy_set_header Host $http_host<span style="color: #66cc66;">;</span>
        proxy_redirect off<span style="color: #66cc66;">;</span>
        proxy_set_header X-Real-IP $remote_addr<span style="color: #66cc66;">;</span>
        proxy_set_header X-Scheme $scheme<span style="color: #66cc66;">;</span>
        proxy_pass http://bapis<span style="color: #66cc66;">;</span>
    <span style="color: black;">&#125;</span>
<span style="color: black;">&#125;</span></pre>
      </td>
    </tr>
  </table>
</div>

Neste momento, se tu seguiu tudo direitinho e for um cara de sorte, já tens o que querias &#8211; quer dizer, o que eu queria &#8211; um **proxy reverso** para meu **Tornado**. 

Eu fiz um teste bobão, assim:

Iniciei duas instancias **Tornado**:

<div class="wp_codebox">
  <table>
    <tr id="p1416250">
      <td class="code" id="p1416code250">
        <pre class="bash" style="font-family:monospace;">python26 <span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>www<span style="color: #000000; font-weight: bold;">/</span>bapi<span style="color: #000000; font-weight: bold;">/</span>main.py <span style="color: #660033;">--port</span>=<span style="color: #000000;">8080</span>
python26 <span style="color: #000000; font-weight: bold;">/</span>var<span style="color: #000000; font-weight: bold;">/</span>www<span style="color: #000000; font-weight: bold;">/</span>bapi<span style="color: #000000; font-weight: bold;">/</span>main.py <span style="color: #660033;">--port</span>=<span style="color: #000000;">8081</span></pre>
      </td>
    </tr>
  </table>
</div>

<del datetime="2012-06-25T18:07:10+00:00">re</del>Iniciei o **nginx**:

<div class="wp_codebox">
  <table>
    <tr id="p1416251">
      <td class="code" id="p1416code251">
        <pre class="bash" style="font-family:monospace;">service nginx restart</pre>
      </td>
    </tr>
  </table>
</div>

E voila, acessei minha aplicação Tornado de um browser qualquer pela porta 8888 e lá estava ela funcionando _belezinha_!

Eu quis complicar um pouquinho e adicionei suporte ao https (SSL), se não tiver afim disto seu trabalho terminou por aqui.

Ops, o teu pode ter terminado aqui, mas o meu não, pois para terminar a implantação deste servidor vou configurar o Supervisor para gerenciar as instancias **Tornado**, quando terminar faço referência da continuação aqui. 

<center>
  <em>==========> A parte do SSL começa aqui <==========</em>
</center>

Como confio em mim, me escolhi como certificador de mim mesmo e gerei meu próprio certificado, assim sem detalhes porque está fora do escopo desta postagen:

<div class="wp_codebox">
  <table>
    <tr id="p1416252">
      <td class="code" id="p1416code252">
        <pre class="bash" style="font-family:monospace;">openssl genrsa <span style="color: #660033;">-out</span> cert.key <span style="color: #000000;">1024</span></pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p1416253">
      <td class="code" id="p1416code253">
        <pre class="bash" style="font-family:monospace;">openssl req <span style="color: #660033;">-new</span> <span style="color: #660033;">-key</span> cert.key <span style="color: #660033;">-out</span> req.pem</pre>
      </td>
    </tr>
  </table>
</div>

<div class="wp_codebox">
  <table>
    <tr id="p1416254">
      <td class="code" id="p1416code254">
        <pre class="bash" style="font-family:monospace;">openssl x509 <span style="color: #660033;">-req</span> <span style="color: #660033;">-days</span> <span style="color: #000000;">364</span> <span style="color: #660033;">-in</span> req.pem <span style="color: #660033;">-signkey</span> cert.key <span style="color: #660033;">-out</span> cert.pem</pre>
      </td>
    </tr>
  </table>
</div></pre> 

Alterei meu /etc/nginx/conf.d/bapi.conf para SSL, veja como ficou:

<div class="wp_codebox">
  <table>
    <tr id="p1416255">
      <td class="code" id="p1416code255">
        <pre class="python" style="font-family:monospace;">proxy_next_upstream error<span style="color: #66cc66;">;</span>
&nbsp;
upstream bapis <span style="color: black;">&#123;</span>
    server 127.0.0.1:<span style="color: #ff4500;">8080</span><span style="color: #66cc66;">;</span>
    server 127.0.0.1:<span style="color: #ff4500;">8081</span><span style="color: #66cc66;">;</span>
<span style="color: black;">&#125;</span>
&nbsp;
server <span style="color: black;">&#123;</span>
    listen       <span style="color: #ff4500;">443</span><span style="color: #66cc66;">;</span>
    ssl on<span style="color: #66cc66;">;</span>
    ssl_certificate /var/www/ssl/cert.<span style="color: black;">pem</span><span style="color: #66cc66;">;</span>
    ssl_certificate_key /var/www/ssl/cert.<span style="color: black;">key</span><span style="color: #66cc66;">;</span>
&nbsp;
    default_type application/octet-stream<span style="color: #66cc66;">;</span>
&nbsp;
    location /static/ <span style="color: black;">&#123;</span>
        root /var/www/static<span style="color: #66cc66;">;</span>
        <span style="color: #ff7700;font-weight:bold;">if</span> <span style="color: black;">&#40;</span>$query_string<span style="color: black;">&#41;</span> <span style="color: black;">&#123;</span>
            expires <span style="color: #008000;">max</span><span style="color: #66cc66;">;</span>
        <span style="color: black;">&#125;</span>
    <span style="color: black;">&#125;</span>
&nbsp;
    location / <span style="color: black;">&#123;</span>
        proxy_pass_header Server<span style="color: #66cc66;">;</span>
        proxy_set_header Host $http_host<span style="color: #66cc66;">;</span>
        proxy_redirect off<span style="color: #66cc66;">;</span>
        proxy_set_header X-Real-IP $remote_addr<span style="color: #66cc66;">;</span>
        proxy_set_header X-Scheme $scheme<span style="color: #66cc66;">;</span>
        proxy_pass http://bapis<span style="color: #66cc66;">;</span>
    <span style="color: black;">&#125;</span>
<span style="color: black;">&#125;</span></pre>
      </td>
    </tr>
  </table>
</div>

Reiniciei as coisas novamente e tudo certo, lá estava meu aplicativo **Tornado** rodando sobre HTTPS.

<center>
  <em>==========> SSL termina aqui <==========</em>
</center>

Valeu