# Cola para configurar uma nova instalção do Manjaro

## Pacotes para instalação

	base-devel vlc qbittorrent speedcrunch deadbeef arduino youtube-dl kicad kicad-library kicad-library-3d visual-studio-code-bin flashplugin python-pipenv otf-fira-code consolas-font powerline-fonts gitkraken nmap micro gimp inkscape htop

## Configurações do sistema

### Mirrorlist

	$ sudo pacman-mirrors --country Brazil Chile && sudo pacman -Syyu

### fstab

Montar automaticamente uma partição. Crie a pasta o ela será montada.

	$ mkdir ~/Arquivos

Guarde o UUID da partição com o comando

	# blkid 
	
Adcionar a seguinte configuração ao arquivo ``/etc/fstab`` e em seguinda remonte todas as partições.

	$ sudo echo -e "UUID=<uuid>\t/home/badaro/Arquivos\tntfs-3g\tdefaults\t0\t0" >> /etc/fstab
	$ sudo mount -a

### Swapfile

Crie e configure o swapfile

	# fallocate -l 4G /swapfile
	# chmod 600 /swapfile
	# mkswap /swapfile
	# swapon /swapfile

Adcione a seguinte linha no arquivo ``/etc/fstab``
	
	/swapfile none swap defaults 0 0
	
Altere a swappness

    $ sudo echo vm.swappiness=10 > /etc/sysctl.d/100-manjaro.conf

### yaourt

Para ignorar a maior parte das confirmações
	
	$ cp /etc/yaourtrc ~/.yaourtrc
	
descomente e mude as opções:

	BUILD_NOCONFIRM=1
	EDITFILES=0

Para habilitar a instalação de pacotes grandes do AUR como o `anaconda` mude a pasta de trabalho padrão do `yaourt` no arquivo de configuração.

    TMPDIR="/home/badaro/.tmp"
    
### Outras configs

(https://forum.manjaro.org/t/top-10-things-to-do-after-a-fresh-install-2018-edition)

## Configurações de pacotes

### Arduino

É necessário adcionar o usuário aos grupos ``lock`` e ``uucp``.

	$ sudo gpasswd -a $USER lock && sudo gpasswd -a $USER uucp

### Oh-my-zsh

Habilitar o zsh como shell padrão

	$ chsh -s $(which zsh)

Script de instalação	

	$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	
agnoster é um tema maneiro, mas precisa de fontes powerline.

Paleta de cores bacana: [https://github.com/tobark/hyper-snazzy-gnome-terminal]

### Anaconda

No arquivo ``~/.profile`` faça a seguinte modficação para tornar o anaconda o pacote secundário de python (normalmente o caminho para o ananconda é ``/opt/anaconda/bin``):

Default configuration of Anaconda installer (deixa o anaconda como pacote primário)

	$ export PATH="/<caminho para o anaconda>/bin:$PATH"

Append Anaconda so that it doesn't override system Python (deixa o anaconda como pacote secundário)

	$ export PATH="$PATH:/<caminho para o anaconda>/bin"

### unified-remote-server

The simplest way to start the server is:

	$ /opt/urserver/urserver --daemon

add the above line to a startup script etc.

There is also a user systemd serice available. You can start it with
	
	# systemctl --user start urserver
	
You can enable at boot with

	# systemctl --user enable urserver
	
(Note: A one time call (as user) to
	
	$ /opt/urserver/urserver-start

could be necessary before using it).For more options:
	
	$ /opt/urserver/urserver --help

See link for port configurations: [Link](http://wiki.unifiedremote.com/wiki/Configuration:Routers_and_Ports)

Configure server from a browser: localhost:9510/web

### KiCad
	
Biblioteca: https://github.com/benwis/SparkFun-Kicad-Libraries

### pacman

Excluir pacores orfãos
	
	$ sudo pacman -Rns $(pacman -Qtdq)

Excluir arquivos não usados

	$ sudo pacman -Sc

### Julia

	$ yaourt -Sy julia

#### Configurações

Para instalar o `PyPlot` é necessário o `matplotlib` do python, se o `anaconda` já está instaldo você pode configar o `PyPlot` para usa-lo definindo 
	
	ENV["PYTHON"] = "<path/to/anaconda>/bin/python"

antes de instalar o `PyCall`, ou pode configurá-lo para baixar o miniconda usando

	ENV["PYTHON"] = ""

Para instalar a biblioteca ``DifferentialEquations`` primeiro instale o pacote do linux ``gcc-fortran``.

.. A bibioleca `Arpack` está com problemas na intalação. Instale o pacote linux `arpack` e copie o lib na pasta da biblioteca.

	$ cp /usr/lib/libarpack.so.2.0.0 /home/badaro/.julia/packages/Arpack/<xxxx>/deps/usr/lib/

#### Pacotes básicos

	]add IJulia Plots PyCall PyPlots LaTeXString DifferentailEquations StaticArrays

### Redshift

Para impedir que o ``geoclue`` peça permissão para pra executar adcione o seguinte no arquivo ``/etc/geoclue/geoclue.conf``

	[redshift]
	allowed=true
	system=false
	users=
