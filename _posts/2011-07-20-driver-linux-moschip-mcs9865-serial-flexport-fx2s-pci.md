---
id: 222
title: 'Driver Moschip MCS9865 &#8211; Serial Flexport FX2S PCI LP/2 para Linux CentOS'
date: 2011-07-20T14:45:03+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=222
permalink: /2011/07/20/driver-linux-moschip-mcs9865-serial-flexport-fx2s-pci/
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
  - driver
  - linux
  - shell
tags:
  - driver
  - flexport
  - fx2s
  - linux
  - mcs9865
  - moschip
  - serial
  - terminal
---
Nestas aventuras no mundo da automação comercial de varejo, novos hardwares para pdv são frequentes, em cada loja nova inaugurada algum novo dispositivo surge e uma nova batalha é traçada para fazer os sistemas legados manterem compatibilidade com estes hardwares.

Usamos o sistema da **Zanthus**, o **Zeus Frente de Loja**, nos pdvs da rede de supermercados do qual trabalho, este sistema possui um instalador baseado no **CentOS** e nele não há módulo adequado para a placa multiserial **FlexPort FX2S PCI LP/2 Serial**, sendo necessário compilar e instalar como módulo do kernel para viabilizar os uso das seriais no sistema.

Para instalar este módulo, pode-se seguir os passos abaixo:

<!--more-->

1- Tenha certeza que a placa serial é mesmo uma **Moschip MCS9865**, com o comando:

<div class="wp_codebox">
  <table>
    <tr id="p22233">
      <td class="code" id="p222code33">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">lspci</span> <span style="color: #660033;">-v</span></pre>
      </td>
    </tr>
  </table>
</div>

A saída deste comando deve listar informações de todas as placas PCI, procure pela **9865**. As informações que você precisa, deverá ser semelhante ao exemplo abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p22234">
      <td class="code" id="p222code34">
        <pre class="bash" style="font-family:monospace;">03:<span style="color: #000000;">01.0</span> Serial controller: NetMos Technology Unknown device <span style="color: #000000;">9865</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>prog-if 02 <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">16550</span><span style="color: #7a0874; font-weight: bold;">&#93;</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
	Subsystem: Unknown device a000:<span style="color: #000000;">1000</span>
	Flags: bus master, medium devsel, latency <span style="color: #000000;">32</span>, IRQ <span style="color: #000000;">209</span>
	I<span style="color: #000000; font-weight: bold;">/</span>O ports at df00 <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=<span style="color: #000000;">8</span><span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Memory at fdeff000 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">32</span>-bit, non-prefetchable<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=4K<span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Memory at fdefe000 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">32</span>-bit, non-prefetchable<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=4K<span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Capabilities: <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">48</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> Power Management version <span style="color: #000000;">2</span>
&nbsp;
03:<span style="color: #000000;">01.1</span> Serial controller: NetMos Technology Unknown device <span style="color: #000000;">9865</span> <span style="color: #7a0874; font-weight: bold;">&#40;</span>prog-if 02 <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">16550</span><span style="color: #7a0874; font-weight: bold;">&#93;</span><span style="color: #7a0874; font-weight: bold;">&#41;</span>
	Subsystem: Unknown device a000:<span style="color: #000000;">1000</span>
	Flags: bus master, medium devsel, latency <span style="color: #000000;">32</span>, IRQ <span style="color: #000000;">217</span>
	I<span style="color: #000000; font-weight: bold;">/</span>O ports at de00 <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=<span style="color: #000000;">8</span><span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Memory at fdefd000 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">32</span>-bit, non-prefetchable<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=4K<span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Memory at fdefc000 <span style="color: #7a0874; font-weight: bold;">&#40;</span><span style="color: #000000;">32</span>-bit, non-prefetchable<span style="color: #7a0874; font-weight: bold;">&#41;</span> <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #007800;">size</span>=4K<span style="color: #7a0874; font-weight: bold;">&#93;</span>
	Capabilities: <span style="color: #7a0874; font-weight: bold;">&#91;</span><span style="color: #000000;">48</span><span style="color: #7a0874; font-weight: bold;">&#93;</span> Power Management version <span style="color: #000000;">2</span></pre>
      </td>
    </tr>
  </table>
</div>

2- Para compilar o módulo é necessário ter o pacote de desenvolvimento do Kernel, para garantir que o seu ambiente atenda estes requisitos, use o comando que segue:

<div class="wp_codebox">
  <table>
    <tr id="p22235">
      <td class="code" id="p222code35">
        <pre class="bash" style="font-family:monospace;">yum <span style="color: #c20cb9; font-weight: bold;">install</span> kernel-devel kernel-headers</pre>
      </td>
    </tr>
  </table>
</div>

3- Baixe o pacote tarball com os fontes do driver, siga o link a seguir e faça o download:

<a href="http://www.shellinux.com.br/v0/downloads/category/1-automacao-comercial.html?download=3%3Amdulo-para-moschip-mcs9865-flexport" target="_blank">msc9865_linux.tar.gz</a>

4- Agora já poderemos compilar e instalar o módulo **MSC9865** para o Kernel, use a sequência de comandos abaixo para descompactar, compilar e instalar:

<div class="wp_codebox">
  <table>
    <tr id="p22236">
      <td class="code" id="p222code36">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">tar</span> zxvf MCS9865_Linux.tar.gz
<span style="color: #7a0874; font-weight: bold;">cd</span> MCS9865_Linux
<span style="color: #c20cb9; font-weight: bold;">make</span>
<span style="color: #c20cb9; font-weight: bold;">make</span> <span style="color: #c20cb9; font-weight: bold;">install</span></pre>
      </td>
    </tr>
  </table>
</div>

5- Após reiniciar a placa multiserial deverá funcionar normalmente, entretanto serão criados dispositivos de caractere no sistema de arquivos fora do padrão comum, como /dev/ttyS0 para porta COM1, então se o seu sistema espera por estes endereços para encontrar as portas seriais será necessário criar links simbólicos que apontem para este sistema de arquivo. Supondo que desejamos liberar as portas seriais da placa **MCS9865** como portas COM3 e COM4, devemos criar os links como nos exemplos abaixo:

<div class="wp_codebox">
  <table>
    <tr id="p22237">
      <td class="code" id="p222code37">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ln</span> <span style="color: #660033;">-sf</span> <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>ttyD0 <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>ttyS2
<span style="color: #c20cb9; font-weight: bold;">ln</span> <span style="color: #660033;">-sf</span> <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>ttyD1 <span style="color: #000000; font-weight: bold;">/</span>dev<span style="color: #000000; font-weight: bold;">/</span>ttyS3</pre>
      </td>
    </tr>
  </table>
</div>

6- Após o comando acima, as portas estarão disponíveis como portas 3 e 4 no sistema, entretanto há casos onde o módulo não é iniciado automaticamente, para evitar este problema, podemos configurar para carregar automaticamente o módulo ao iniciar o computador nos níveis inicialização mais usados, o nível 3 (shell com multiusuário) e o nível 5 (X com multiusuário). Para configurar desta forma podemos usar os comandos que seguem:

<div class="wp_codebox">
  <table>
    <tr id="p22238">
      <td class="code" id="p222code38">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"modprobe mcs9865"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>mcs9865
<span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"modprobe mcs9865-isa"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>mcs9865</pre>
      </td>
    </tr>
  </table>
</div>

7- Acerte a permissão do arquivo criado para permitir a execução dele:

<div class="wp_codebox">
  <table>
    <tr id="p22239">
      <td class="code" id="p222code39">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">chmod</span> +x <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>mcs9865</pre>
      </td>
    </tr>
  </table>
</div>

8- Crie os links simbólicos para iniciar no nível 3 e 5:

<div class="wp_codebox">
  <table>
    <tr id="p22240">
      <td class="code" id="p222code40">
        <pre class="bash" style="font-family:monospace;"><span style="color: #c20cb9; font-weight: bold;">ln</span> <span style="color: #660033;">-s</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>mcs9865 <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>rc.d<span style="color: #000000; font-weight: bold;">/</span>rc3.d<span style="color: #000000; font-weight: bold;">/</span>Smcs9865 <span style="color: #000000; font-weight: bold;">||</span> <span style="color: #c20cb9; font-weight: bold;">true</span>
<span style="color: #c20cb9; font-weight: bold;">ln</span> <span style="color: #660033;">-s</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>init.d<span style="color: #000000; font-weight: bold;">/</span>mcs9865 <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>rc.d<span style="color: #000000; font-weight: bold;">/</span>rc5.d<span style="color: #000000; font-weight: bold;">/</span>Smcs9865 <span style="color: #000000; font-weight: bold;">||</span> <span style="color: #c20cb9; font-weight: bold;">true</span></pre>
      </td>
    </tr>
  </table>
</div>

9- Por fim devemos incluir no **rc.local**, com os comandos abaixo, as linhas que fazem os links simbólicos para os novos dispositivos criados (no exemplo abaixo as portas serão disponibilizadas no sistema como portas COM3 e COM4):

<div class="wp_codebox">
  <table>
    <tr id="p22241">
      <td class="code" id="p222code41">
        <pre class="bash" style="font-family:monospace;"><span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"/bin/ln -sf /dev/ttyD0 /dev/ttyS2"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>rc.local
<span style="color: #7a0874; font-weight: bold;">echo</span> <span style="color: #ff0000;">"/bin/ln -sf /dev/ttyD1 /dev/ttyS3"</span> <span style="color: #000000; font-weight: bold;">&gt;&gt;</span> <span style="color: #000000; font-weight: bold;">/</span>etc<span style="color: #000000; font-weight: bold;">/</span>rc.local</pre>
      </td>
    </tr>
  </table>
</div>