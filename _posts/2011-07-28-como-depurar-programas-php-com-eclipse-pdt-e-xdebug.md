---
id: 289
title: Como depurar código PHP com Eclipse, PDT e XDebug
date: 2011-07-28T14:18:44+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=289
permalink: /2011/07/28/como-depurar-programas-php-com-eclipse-pdt-e-xdebug/
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
categories:
  - automação
  - imagens
  - linux
  - shell
tags:
  - debug
  - eclipse
  - fedora
  - linux
  - pdt
  - php
  - xdebug
---
Continuando a configuração do meu Fedora 15, segue como fiz para conseguir depurar programas PHP.

Numa _tacada_ só vamos instalar todos os programas necessários para depurar programas PHP no Eclipse usando PDT e XDebug em um ambiente recém instalado com o Fedora 15 Live CD, sem _segredinhos_ ou pacotes extras é a instalação padrão mesmo.

Para tal execute o comando abaixo com o usuário **root**, que ira instalar o Eclipse, o Apache, o MySQL, o PHP com suporte ao XDebug e MySQL:

<!--more-->

<div class="wp_codebox">
  <table>
    <tr id="p28948">
      <td class="code" id="p289code48">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> eclipse mysql-server mysql phpmyadmin php httpd php-devel php-pecl-xdebug</pre>
      </td>
    </tr>
  </table>
</div>

Ao término da instalação, um arquivo **xdebug.ini** deverá ter sido criado em **/etc/php.d** como este do exemplo abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p28949">
      <td class="code" id="p289code49">
        <pre class="bash" style="font-family:monospace;">; Enable xdebug extension module
<span style="color: #007800;">zend_extension</span>=<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>php<span style="color: #000000; font-weight: bold;">/</span>modules<span style="color: #000000; font-weight: bold;">/</span>xdebug.so</pre>
      </td>
    </tr>
  </table>
</div>

Mas ainda falta alguns parâmetros para deixa-lo _redondinho_, deixe seu arquivo igual ao conteúdo abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p28950">
      <td class="code" id="p289code50">
        <pre class="bash" style="font-family:monospace;">; Enable xdebug extension module
<span style="color: #007800;">zend_extension</span>=<span style="color: #000000; font-weight: bold;">/</span>usr<span style="color: #000000; font-weight: bold;">/</span>lib<span style="color: #000000; font-weight: bold;">/</span>php<span style="color: #000000; font-weight: bold;">/</span>modules<span style="color: #000000; font-weight: bold;">/</span>xdebug.so
xdebug.remote_enable=<span style="color: #000000;">1</span>
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
xdebug.remote_host=localhost
xdebug.remote_port=<span style="color: #000000;">9100</span>
xdebug.extended_info=<span style="color: #000000;">1</span></pre>
      </td>
    </tr>
  </table>
</div>

Para facilitar a depuração dos arquivos é interessante configurar para que o Apache utilize o recurso **UserDir**, assim todos os seus projetos php poderão ficar abaixo da pasta **home** do seu usuário, local onde normalmente criamos a pasta **workspaces** do Eclipse, depois veremos como fiz isto.

Agora vamos habilitar o UserDir no http.conf, para tal precisamos edita-lo e alterá-lo, no Fedora 15 ele está na pasta /etc/httpd/conf/.
  
Procure por **mod_userdir** e comente a linha **UserDir disable** e _des_comente a linha **UserDir public_html** trocando o local de **public_html** para **workspaces**. Sugiro colocar este local, porque depois vamos definir como pasta padrão do Eclipse o caminho **/home/[Meu Usuário]/workspaces**.

Seu bloco de configuração do **UserDir** no **httpd.conf** deve ficar como este:

<div class="wp_codebox">
  <table>
    <tr id="p28951">
      <td class="code" id="p289code51">
        <pre class="bash" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">&lt;</span>IfModule mod_userdir.c<span style="color: #000000; font-weight: bold;">&gt;</span>
    <span style="color: #666666; font-style: italic;">#UserDir disabled</span>
    UserDir workspaces
<span style="color: #000000; font-weight: bold;">&lt;/</span>IfModule<span style="color: #000000; font-weight: bold;">&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Reinicie o Apache:

<div class="wp_codebox">
  <table>
    <tr id="p28952">
      <td class="code" id="p289code52">
        <pre class="bash" style="font-family:monospace;">service httpd restart</pre>
      </td>
    </tr>
  </table>
</div>

Abra o Eclipse.

<div class="wp_codebox">
  <table>
    <tr id="p28953">
      <td class="code" id="p289code53">
        <pre class="bash" style="font-family:monospace;">eclipse</pre>
      </td>
    </tr>
  </table>
</div>

Se é a primeira vez que você faz isto uma tela perguntando qual o espaço de trabalho deverá ser utilizado deve ser apresentada (figura abaixo), aponte este local para **/home/[Meu Usuário]/workspaces** e marque a opção para **Utilizar como padrão e não perguntar novamente** se julgar conveniente.

[<img src="https://i2.wp.com/farm7.staticflickr.com/6028/5984982596_eb6ee25d1a_z.jpg?resize=640%2C372&#038;ssl=1" alt="Espaco Trabalho Inicialização Eclipse" width="640" height="372" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5984982596/)

Caso você já tenha definido algum outro caminho, você pode acessar o menu **Janelas -> Preferências** e depois **Geral -> Iniciar e Encerrar -> Workspaces** e trocar o local. Veja figura:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6150/5984996542_c31c746ff0_z.jpg?resize=640%2C288&#038;ssl=1" alt="Alterando o espaço de trabalho Eclipse" width="640" height="288" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5984996542/)

Ainda no Eclipse, vamos ajustar alguns parâmetros para poder depurar os programas.
  
Agora acesse o menu **Janelas -> Preferências** e depois **PHP -> Debug -> Installed Debuggers**. Nesta tela, conforme figura a seguir, selecione **XDebug** e clique em **Configure**, uma nova tela será aberta, defina no parâmetro **Debug Port** o valor **9100**, observe que deve ser a mesma porta que definimos no arquivo **xdebug.ini** citado anteriormente.

[<img src="https://i2.wp.com/farm7.staticflickr.com/6144/5982220799_145847fce3_z.jpg?resize=640%2C360&#038;ssl=1" alt="xdebug1" width="640" height="360" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5982220799/)

Ainda no menu **Preferências**, acesse **PHP -> PHP Executables** selecione o aplicativo **PHP (Workspace Default)**, se existir e clique em **Edit&#8230;** ou adicione um se não houver, clicando em **Add&#8230;**, a tela citada deve ser como esta da figura a seguir:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6027/5982220767_14581069d8_z.jpg?resize=640%2C360&#038;ssl=1" alt="xdebug3_" width="640" height="360" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5982220767/)

Defina o executável do PHP, no meu Fedora 15 o caminho é **/usr/bin/php**, dê um nome, sugiro **PHP** mesmo, selecione **CLI** na opção **SAPI Type** e **XDebug** no parâmetro **PHP Debugger**, conforme figura abaixo:

[<img src="https://i2.wp.com/farm7.staticflickr.com/6125/5982220773_bdd8cc8753_z.jpg?resize=640%2C360&#038;ssl=1" alt="xdebug3" width="640" height="360" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5982220773/)

Pronto de se tudo correu bem, já temos um Eclipse com depurador PHP, vamos testar?

Primeiro teste a configuração do **UserDir** do Apache, para isto crie um arquivo com o nome **index.php** dentro da pasta **/home/[Meu Usuario]/workspaces**, com o seguinte conteúdo:

<div class="wp_codebox">
  <table>
    <tr id="p28954">
      <td class="code" id="p289code54">
        <pre class="php" style="font-family:monospace;"><span style="color: #000000; font-weight: bold;">&lt;?php</span>
<a href="http://www.php.net/phpinfo"><span style="color: #990000;">phpinfo</span></a><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
<span style="color: #000000; font-weight: bold;">?&gt;</span></pre>
      </td>
    </tr>
  </table>
</div>

Certifique-se de que as permissões da pasta **/home/[Meu Usuario]** é 711 com o comando:

<div class="wp_codebox">
  <table>
    <tr id="p28955">
      <td class="code" id="p289code55">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chmod</span> <span style="color: #660033;">-Rf</span> <span style="color: #000000;">711</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span><span style="color: #7a0874; font-weight: bold;">&#91;</span>Meu Usuario<span style="color: #7a0874; font-weight: bold;">&#93;</span></pre>
      </td>
    </tr>
  </table>
</div>

E que os arquivos da **workspaces** estão com a permissão 755 usando:

<div class="wp_codebox">
  <table>
    <tr id="p28956">
      <td class="code" id="p289code56">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chmod</span> <span style="color: #660033;">-Rf</span> <span style="color: #000000;">755</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span><span style="color: #7a0874; font-weight: bold;">&#91;</span>Meu Usuario<span style="color: #7a0874; font-weight: bold;">&#93;</span><span style="color: #000000; font-weight: bold;">/</span>workspaces</pre>
      </td>
    </tr>
  </table>
</div>

Ok, agora no browser podemos acessar usando o endereço **http://localhost/~[Meu Usuario]/index.php**, se tudo correu bem a saída da função **phpinfo()** deve ser exibida.

Pronto, vá no seu projeto ou arquivo PHP e clique com o botão direito e selecione **Depurar como** e depois **Depurar Configurations**, uma tela como esta a seguir deve ser apresentada:

[<img src="https://i1.wp.com/farm7.staticflickr.com/6029/5985164460_9372dac7e1_z.jpg?resize=640%2C452&#038;ssl=1" alt="depurar como eclipse php" width="640" height="452" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5985164460/)

Nela clique duas vezes em **PHP Web Page** ou clique uma única vez e depois clique em novo para que possamos criar uma configuração para depurar este projeto/arquivo PHP. Na configuração você deve dar uma nome de sua preferência no campo **Nome**, selecionar **XDebug** no parâmetro **Server Debugger**, no **PHP Server** deixar o default, no campo **File** clique em **Browse** e selecione o arquivo principal do seu projeto e atente ao detalhe a seguir.
  
Como definimos o parâmetro UserDir, para funcionar _direitinho_ precisamos dizer para a configuração do debug a URL correta, isto pode ser feito alterando o parâmetro URL, para tal desmarque a opção **Auto Generate** e coloque antes da URL gerada automaticamente o caminho do seu usuário precedido do &#8220;~&#8221;, conforme figura abaixo: 

[<img src="https://i1.wp.com/farm7.staticflickr.com/6135/5984615013_7b8406978c_z.jpg?resize=580%2C640&#038;ssl=1" alt="Configuração do debug Eclipse PHP" width="580" height="640" data-recalc-dims="1" />](http://www.flickr.com/photos/nayamonia/5984615013/) 

Aplique as configurações e feche esta janela. Agora você pode clicar com o botão direito no seu arquivo/projeto PHP e selecionar **Depurar como -> PHP Web Page**, pronto já podemos debugar, claro não esqueça de colocar algum breakpoint no projeto, caso contrário ele não vai parar em lugar nenhum. Para marcar breakpoints de um duplo clique ao lado esquerdo da linha que deseja parar.

Por fim, até que da um trabalhinho configurar, mas vale a pena, pois usar o **echo** para encontrar falhas é pra matar o programador.