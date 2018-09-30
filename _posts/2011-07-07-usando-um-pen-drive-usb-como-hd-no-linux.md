---
id: 156
title: Usando um pen drive USB como HD no Linux
date: 2011-07-07T19:24:04+00:00
author: Gabriel Fernandes
layout: post
guid: http://gabrielf.com.br/wp0/?p=156
permalink: /2011/07/07/usando-um-pen-drive-usb-como-hd-no-linux/
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
  - linux
  - shell
  - virtualização
tags:
  - hd
  - linux
  - pen drive
  - usb
---
Contrário ao que a maioria das pessoas pensam ao ler o título deste post, colocar um sistema Linux para rodar em um pen drive usb e usá-lo como o disco rígido principal, não é algo tão difícil.

Estamos falando de um sistema Linux instalado no pen drive e não um sistema Live Linux, pois este último, temos muitos sistemas prontos e disponíveis na rede.

Em resumo, para rodarmos um sistema Linux em um pen drive USB como HD, você deve instalar normalmente o sistema no pen drive, inclusive o Grub. Depois configurar no SETUP do computador para iniciar o Boot no pen drive USB, agora &#8220;Disco Rígido USB&#8221; e deverá funcionar.

Muito fácil né. Pena que a vida não é um mar de rosas. Prepare-se para os detalhes. 

<!--more-->

Isto pode não funcionar porque os dispositivos USB são reconhecidos e disponibilizados sempre após os discos rígidos comuns (Sata, Pata, SCSI, SAS,&#8230;), portanto se você instalou o sistema no Pen Drive e o seu computador possui um disco rígido interno, todas as entradas dos pontos de montagem colocadas no arquivo de configuração do Boot Loader, no caso do Grub o arquivo **/boot/grub.conf** e no arquivo da tabela de sistema de arquivos, o **/etc/fstab**, estarão apontando para <del datetime="2011-07-07T21:28:20+00:00">provavelmente</del> o dispositivo **/dev/sda**, primeiro disco rígido em um sistema Linux, e quando você colocou para iniciar o boot no disco USB este será disponibilizado como **/dev/sdb**, ou seja, todas as montagens de discos irão falhar, causando a quebra do sistema. Fique atento, pois sda e sdb são referências, na sua instalação pode ser diferente, mas o importante é entender que o sistema só não irá subir porque há divergência nos pontos de montagens do momento da instalação para o da execução.

Uma solução, não muito bela, é remover fisicamente o disco rígido interno e refazer a instalação do sistema no Pen Drive, pois neste caso o instalador do Linux irá reconhecer apenas um disco e ao inicializar tudo deve funcionar normalmente. Entretanto há uma solução muito mais poética que esta, no qual consiste em ajustar estas entradas nos arquivos **/etc/fstab** e **/boot/grub.conf**.

De posse do Pen Drive já instalado com o sistema Linux, conecte ele em qualquer computador, no qual tenha um sistema Linux funcionando e acesse o conteúdo do Pen Drive para editar os arquivos citados anteriormente. No arquivo **/boot/grub.conf** procure pela expressão **root=** e acerte o valor, por exemplo, digamos que esteja com o valor **/dev/sda1** e você tem um disco rígido interno, coloque para **/dev/sdb1** e no **/etc/fstab** siga a lógica da alteração no **/boot/grub.conf**, ou seja, se trocamos a entrada no Grub de **/dev/sdaX** para **/dev/sdbX**, faça o mesmo em todas as linhas do fstab que contiverem o dispositivo em questão.

Por fim, ter um sistema Linux instalado e funcionando em um Pen Drive requer um pouco mais de atenção do usuário logo após instalá-lo acerca da quantidade e ordem dos discos no computador, pois fora isto, não há impedimento para tal funcionalidade.