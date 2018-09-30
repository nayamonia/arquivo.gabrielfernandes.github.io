---
id: 1804
title: Como importar máquina virtual por linha de comando no VirtualBox
date: 2014-04-18T13:05:34+00:00
author: Gabriel Fernandes
layout: post
guid: http://www.gabrielfernandes.org/?p=1804
permalink: /2014/04/18/como-importar-maquina-virtual-por-linha-de-comando-no-virtualbox/
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
image: /wp-content/uploads/2014/03/maquinas.png
categories:
  - virtualização
tags:
  - linha de comando
  - máquina virtual
  - virtualbox
---
O <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a> tem o poder de importar <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> previamente exportada no padrão industrial Open Virtualization Format (OVF), se você quer saber como exportar uma <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> no <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a> clique [**aqui**](http://www.gabrielfernandes.org/2014/03/12/como-exportar-maquina-virtual-por-linha-de-comando-no-virtualbox/ "Clique agora e veja como exportar máquina virtual por linha de comando no VirtualBox").

> O OVF é um padrão aceito na maioria das soluções de virtualização e é normatizado pela Distributed Management Task Force (DMTF). Para mais informações sobre o OVF clique <a title="Site em inglês com a especificação do OVF" href="http://www.dmtf.org/standards/ovf" target="_blank">aqui</a>

Utilizaremos o subcomando **import** do **VBoxManage** para importar <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> por linha de comando no <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a>, acompanhe comigo:<!--more [CONTINUAR LENDO]-->

Na hora de importar podemos decidir o que importar, trocar configurações, ou seja, tem diversão garantida! Vou explicar&#8230;

  * Para explorar o que há no arquivo OVF, ou seja, para sabermos informações do appliance que foi exportado, usamos o comando **VBoxManage**, seguido do subcomando **import**, **caminho completo do arquivo OVF** e a opção **&#8211;dry-run** (os preguiçosos podem usar a opção resumida **-n**). A sintaxe básica do comando é: <div class="wp_codebox">
      <table>
        <tr id="p1804266">
          <td class="code" id="p1804code266">
            <pre class="bash" style="font-family:monospace;">VBoxManage import caminho<span style="color: #000000; font-weight: bold;">/</span>completo<span style="color: #000000; font-weight: bold;">/</span>do<span style="color: #000000; font-weight: bold;">/</span>arquivo_OVF.ova <span style="color: #660033;">-n</span></pre>
          </td>
        </tr>
      </table>
    </div>

  * Ou assim: <div class="wp_codebox">
      <table>
        <tr id="p1804267">
          <td class="code" id="p1804code267">
            <pre class="bash" style="font-family:monospace;">VBoxManage import caminho<span style="color: #000000; font-weight: bold;">/</span>completo<span style="color: #000000; font-weight: bold;">/</span>do<span style="color: #000000; font-weight: bold;">/</span>arquivo_OVF.ova <span style="color: #660033;">--dry-run</span></pre>
          </td>
        </tr>
      </table>
    </div>

  * Vou fazer um exemplo prático com um appliance de verdade, exportado anteriormente seguindo este outro post [**aqui**](http://www.gabrielfernandes.org/2014/03/12/como-exportar-maquina-virtual-por-linha-de-comando-no-virtualbox/ "Clique agora e veja como exportar máquina virtual por linha de comando no VirtualBox"). O nome do arquivo OVF é **exp\_desktop\_XP.ova**, então o comando ficará assim: <div class="wp_codebox">
      <table>
        <tr id="p1804268">
          <td class="code" id="p1804code268">
            <pre class="bash" style="font-family:monospace;">$ VBoxManage import exp_desktop_XP.ova <span style="color: #660033;">-n</span></pre>
          </td>
        </tr>
      </table>
    </div><figure id="attachment_1781" style="width: 967px" class="wp-caption aligncenter">

[<img src="https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/maquinas.png?resize=660%2C635" alt=Imagem da interface gráfica do VirtualBox, com a máquina virtual já importada, continue lendo e veja como chegar até aqui" width="660" height="635" class="size-full wp-image-1781" srcset="https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/maquinas.png?w=967 967w, https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/maquinas.png?resize=300%2C288 300w" sizes="(max-width: 660px) 100vw, 660px" data-recalc-dims="1" />](https://i2.wp.com/www.gabrielfernandes.org/wp-content/uploads/2014/03/maquinas.png)<figcaption class="wp-caption-text">Imagem da interface gráfica do <a href="http://www.gabrielfernandes.org/tag/virtualbox/" target="_blank"><strong>VirtualBox</strong></a>, com a máquina virtual já importada, continue lendo e veja como chegar até aqui</figcaption></figure> 

  * Perai, perai! Olha só que belezinha, este comando deve mostrar pra ti informações parecidas com estas abaixo. São informações do hardware e sistema instalado na máquina virtual que está neste OVF. Curte só, que depois vou explicar. <div class="wp_codebox">
      <table>
        <tr id="p1804269">
          <td class="code" id="p1804code269">
            <pre class="bash" style="font-family:monospace;">$ VBoxManage import exp_desktop_XP.ova <span style="color: #660033;">-n</span>
<span style="color: #000000;"></span><span style="color: #000000; font-weight: bold;">%</span>...10<span style="color: #000000; font-weight: bold;">%</span>...20<span style="color: #000000; font-weight: bold;">%</span>...30<span style="color: #000000; font-weight: bold;">%</span>...40<span style="color: #000000; font-weight: bold;">%</span>...50<span style="color: #000000; font-weight: bold;">%</span>...60<span style="color: #000000; font-weight: bold;">%</span>...70<span style="color: #000000; font-weight: bold;">%</span>...80<span style="color: #000000; font-weight: bold;">%</span>...90<span style="color: #000000; font-weight: bold;">%</span>...100<span style="color: #000000; font-weight: bold;">%</span>
Interpreting <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>exp_desktop_XP.ova...
OK.
Disks:  vmdisk1	<span style="color: #000000;">10544480256</span>	<span style="color: #660033;">-1</span>	http:<span style="color: #000000; font-weight: bold;">//</span>www.vmware.com<span style="color: #000000; font-weight: bold;">/</span>interfaces<span style="color: #000000; font-weight: bold;">/</span>specifications<span style="color: #000000; font-weight: bold;">/</span>vmdk.html<span style="color: #666666; font-style: italic;">#streamOptimized	exp_desktop_XP-disk1.vmdk	-1	-1	</span>
Virtual system <span style="color: #000000;"></span>:
 <span style="color: #000000;"></span>: Suggested OS <span style="color: #7a0874; font-weight: bold;">type</span>: <span style="color: #ff0000;">"WindowsXP"</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --ostype "</span>; use <span style="color: #ff0000;">"list ostypes"</span> to list all possible values<span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">1</span>: Suggested VM name <span style="color: #ff0000;">"desktop_XP_1"</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --vmname "</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">2</span>: Number of CPUs: <span style="color: #000000;">1</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --cpus "</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">3</span>: Guest memory: <span style="color: #000000;">512</span> MB
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --memory "</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">4</span>: Sound card <span style="color: #7a0874; font-weight: bold;">&#40;</span>appliance expects <span style="color: #ff0000;">""</span>, can change on import<span style="color: #7a0874; font-weight: bold;">&#41;</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 4 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">5</span>: USB controller
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 5 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">6</span>: Network adapter: orig NAT, config <span style="color: #000000;">2</span>, extra <span style="color: #007800;">slot</span>=<span style="color: #000000;"></span>;<span style="color: #007800;">type</span>=NAT
 <span style="color: #000000;">7</span>: CD-ROM
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 7 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">8</span>: IDE controller, <span style="color: #7a0874; font-weight: bold;">type</span> PIIX4
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 8 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">9</span>: IDE controller, <span style="color: #7a0874; font-weight: bold;">type</span> PIIX4
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 9 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
<span style="color: #000000;">10</span>: Hard disk image: <span style="color: #7a0874; font-weight: bold;">source</span> <span style="color: #007800;">image</span>=exp_desktop_XP-disk1.vmdk, target <span style="color: #007800;">path</span>=<span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>exp_desktop_XP-disk1.vmdk, <span style="color: #007800;">controller</span>=<span style="color: #000000;">8</span>;<span style="color: #007800;">channel</span>=<span style="color: #000000;"></span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change target path with <span style="color: #ff0000;">"--vsys 0 --unit 10 --disk path"</span>;
    disable with <span style="color: #ff0000;">"--vsys 0 --unit 10 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span></pre>
          </td>
        </tr>
      </table>
    </div>
    
      * Vou comentar algumas coisas interessantes que podemos observar nestas informações. Este arquivo OVF contem apenas uma máquina virtual, podemos ver isto porque há apenas o _Virtual system 0:_, isto é importante porque quando formos importar este appliance devemos informar qual sistema virtual, em nosso caso será o &#8220;0&#8221; e para referenciar ele na linha de comando de importação usaremos o parametro _&#8211;vsys_, a sintaxe é mais ou menos assim para o _Virtual system 0:_: <div class="wp_codebox">
          <table>
            <tr id="p1804270">
              <td class="code" id="p1804code270">
                <pre class="bash" style="font-family:monospace;">VBoxManage import caminho<span style="color: #000000; font-weight: bold;">/</span>completo<span style="color: #000000; font-weight: bold;">/</span>do<span style="color: #000000; font-weight: bold;">/</span>arquivo_OVF.ova <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span></pre>
              </td>
            </tr>
          </table>
        </div>
    
      * Já vimos como identificar o sistema que vamos importar, agora se você quer importar alterando alguns parâmetros, basta observar o índice da configuração nas informações do appliance e referenciá-lo na linha de comando trocando o que você desejar. Vou explicar com um exemplo: 
        Eu quero alterar o nome da máquina virtual, o caminho do disco virtual de _/home/gabriel/VMs/conf/desktop\_XP\_1/exp\_desktop\_XP-disk1.vmdk_ para _/home/gabriel/VMs/conf/desktop\_XP\_1/Windows\_XP\_imp.vmdk_ e também ignorar a segunda controladora _IDE controller, type PIIX4_. 
        Olhando para as informações do appliance, podemos identificar que o nome da máquina virtual é o parâmetro de indice &#8220;1&#8221;, controladora é o &#8220;9&#8221; e o disco é o &#8220;10&#8221; no sistema virtual &#8220;0&#8221; .</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804271">
              <td class="code" id="p1804code271">
                <pre class="bash" style="font-family:monospace;"> <span style="color: #000000;">1</span>: Suggested VM name <span style="color: #ff0000;">"desktop_XP_1"</span>
...
 <span style="color: #000000;">9</span>: IDE controller, <span style="color: #7a0874; font-weight: bold;">type</span> PIIX4
...
<span style="color: #000000;">10</span>: Hard disk image: <span style="color: #7a0874; font-weight: bold;">source</span> <span style="color: #007800;">image</span>=exp_desktop_XP-disk1.vmdk, target <span style="color: #007800;">path</span>=<span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>exp_desktop_XP-disk1.vmdk, <span style="color: #007800;">controller</span>=<span style="color: #000000;">8</span>;<span style="color: #007800;">channel</span>=<span style="color: #000000;"></span>
...</pre>
              </td>
            </tr>
          </table>
        </div>
        
        Para trocar o nome, indicamos o _Virtual System 0_, seguido do parâmetro _&#8211;vname_, mais o novo nome da máquina entre aspas duplas, é algo mais ou menos assim:</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804272">
              <td class="code" id="p1804code272">
                <pre class="bash" style="font-family:monospace;">... <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span> <span style="color: #660033;">--vmname</span> <span style="color: #ff0000;">"Novo Nome da VM"</span> ...</pre>
              </td>
            </tr>
          </table>
        </div>
        
        Agora para ignorar uma das duas controladoras do _Virtual System 0_, vamos usar algo assim na linha de comando:</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804273">
              <td class="code" id="p1804code273">
                <pre class="bash" style="font-family:monospace;">... <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span> <span style="color: #660033;">--unit</span> <span style="color: #000000;">9</span> <span style="color: #660033;">--ignore</span> ...</pre>
              </td>
            </tr>
          </table>
        </div>
        
        Por fim, para alterar o caminho do disco, algo como o trecho abaixo, vai resolver:</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804274">
              <td class="code" id="p1804code274">
                <pre class="bash" style="font-family:monospace;">... <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span> <span style="color: #660033;">--unit</span> <span style="color: #000000;">10</span> <span style="color: #660033;">--disk</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>Windows_XP_imp.vmdk ...</pre>
              </td>
            </tr>
          </table>
        </div>
        
        O comando final, ou seja, tudo juntinho e misturado como eu usei aqui é:</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804275">
              <td class="code" id="p1804code275">
                <pre class="bash" style="font-family:monospace;">$ VBoxManage import exp_desktop_XP.ova <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span> <span style="color: #660033;">--vmname</span> <span style="color: #ff0000;">"Windows_XP_imp"</span> <span style="color: #660033;">--unit</span> <span style="color: #000000;">10</span> <span style="color: #660033;">--disk</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>Windows_XP_imp.vmdk <span style="color: #660033;">--unit</span> <span style="color: #000000;">9</span> <span style="color: #660033;">--ignore</span></pre>
              </td>
            </tr>
          </table>
        </div>
        
        Se tudo estiver dando certo, como foi comigo aqui, você deve estar vendo no terminal mensagens como estas abaixo. Curta e seja feliz!</p> 
        
        <div class="wp_codebox">
          <table>
            <tr id="p1804276">
              <td class="code" id="p1804code276">
                <pre class="bash" style="font-family:monospace;">$ VBoxManage import exp_desktop_XP.ova <span style="color: #660033;">--vsys</span> <span style="color: #000000;"></span> <span style="color: #660033;">--vmname</span> <span style="color: #ff0000;">"Windows_XP_imp"</span> <span style="color: #660033;">--unit</span> <span style="color: #000000;">10</span> <span style="color: #660033;">--disk</span> <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>Windows_XP_imp.vmdk <span style="color: #660033;">--unit</span> <span style="color: #000000;">9</span> <span style="color: #660033;">--ignore</span> 
<span style="color: #000000;"></span><span style="color: #000000; font-weight: bold;">%</span>...10<span style="color: #000000; font-weight: bold;">%</span>...20<span style="color: #000000; font-weight: bold;">%</span>...30<span style="color: #000000; font-weight: bold;">%</span>...40<span style="color: #000000; font-weight: bold;">%</span>...50<span style="color: #000000; font-weight: bold;">%</span>...60<span style="color: #000000; font-weight: bold;">%</span>...70<span style="color: #000000; font-weight: bold;">%</span>...80<span style="color: #000000; font-weight: bold;">%</span>...90<span style="color: #000000; font-weight: bold;">%</span>...100<span style="color: #000000; font-weight: bold;">%</span>
Interpreting <span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>exp_desktop_XP.ova...
OK.
Disks:  vmdisk1	<span style="color: #000000;">10544480256</span>	<span style="color: #660033;">-1</span>	http:<span style="color: #000000; font-weight: bold;">//</span>www.vmware.com<span style="color: #000000; font-weight: bold;">/</span>interfaces<span style="color: #000000; font-weight: bold;">/</span>specifications<span style="color: #000000; font-weight: bold;">/</span>vmdk.html<span style="color: #666666; font-style: italic;">#streamOptimized	exp_desktop_XP-disk1.vmdk	-1	-1	</span>
Virtual system <span style="color: #000000;"></span>:
 <span style="color: #000000;"></span>: Suggested OS <span style="color: #7a0874; font-weight: bold;">type</span>: <span style="color: #ff0000;">"WindowsXP"</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --ostype "</span>; use <span style="color: #ff0000;">"list ostypes"</span> to list all possible values<span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">1</span>: VM name specified with --vmname: <span style="color: #ff0000;">"Windows_XP_imp"</span>
 <span style="color: #000000;">2</span>: Number of CPUs: <span style="color: #000000;">1</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --cpus "</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">3</span>: Guest memory: <span style="color: #000000;">512</span> MB
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>change with <span style="color: #ff0000;">"--vsys 0 --memory "</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">4</span>: Sound card <span style="color: #7a0874; font-weight: bold;">&#40;</span>appliance expects <span style="color: #ff0000;">""</span>, can change on import<span style="color: #7a0874; font-weight: bold;">&#41;</span>
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 4 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">5</span>: USB controller
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 5 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">6</span>: Network adapter: orig NAT, config <span style="color: #000000;">2</span>, extra <span style="color: #007800;">slot</span>=<span style="color: #000000;"></span>;<span style="color: #007800;">type</span>=NAT
 <span style="color: #000000;">7</span>: CD-ROM
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 7 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">8</span>: IDE controller, <span style="color: #7a0874; font-weight: bold;">type</span> PIIX4
    <span style="color: #7a0874; font-weight: bold;">&#40;</span>disable with <span style="color: #ff0000;">"--vsys 0 --unit 8 --ignore"</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
 <span style="color: #000000;">9</span>: IDE controller, <span style="color: #7a0874; font-weight: bold;">type</span> PIIX4 <span style="color: #660033;">--</span> disabled
<span style="color: #000000;">10</span>: Hard disk image: <span style="color: #7a0874; font-weight: bold;">source</span> <span style="color: #007800;">image</span>=exp_desktop_XP-disk1.vmdk, target <span style="color: #007800;">path</span>=<span style="color: #000000; font-weight: bold;">/</span>home<span style="color: #000000; font-weight: bold;">/</span>gabriel<span style="color: #000000; font-weight: bold;">/</span>VMs<span style="color: #000000; font-weight: bold;">/</span>conf<span style="color: #000000; font-weight: bold;">/</span>desktop_XP_1<span style="color: #000000; font-weight: bold;">/</span>Windows_XP_imp.vmdk, <span style="color: #007800;">controller</span>=<span style="color: #000000;">8</span>;<span style="color: #007800;">channel</span>=<span style="color: #000000;"></span>
<span style="color: #000000;"></span><span style="color: #000000; font-weight: bold;">%</span>...10<span style="color: #000000; font-weight: bold;">%</span>...20<span style="color: #000000; font-weight: bold;">%</span>...30<span style="color: #000000; font-weight: bold;">%</span>...40<span style="color: #000000; font-weight: bold;">%</span>...50<span style="color: #000000; font-weight: bold;">%</span>...60<span style="color: #000000; font-weight: bold;">%</span>...70<span style="color: #000000; font-weight: bold;">%</span>...80<span style="color: #000000; font-weight: bold;">%</span>...90<span style="color: #000000; font-weight: bold;">%</span>...100<span style="color: #000000; font-weight: bold;">%</span>
Successfully imported the appliance.</pre>
              </td>
            </tr>
          </table>
        </div></ul> 
    
    Se você quer aprender a subir esta <a href="http://www.gabrielfernandes.org/tag/maquina-virtual/" target="_blank"><strong>máquina virtual</strong></a> recém importada pela linha de comando, espia este outro post [**aqui**](http://www.gabrielfernandes.org/2011/09/02/criar-maquina-virtual-no-virtualbox-pela-linha-de-comando/ "Clique e aprenda mais sobre VirtualBox na linha de comando"), lá no finalzinho dele mostra como fazer.
    
    Dúvidas? Pergunte direto pra mim!
    
    Valeu!