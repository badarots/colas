# Cola para configurar uma nova instalção do Manjaro

## Pacotes para instalação

vlc qbittorrent speedcrunch deadbeef arduino youtube-dl kicad kicad-library kicad-library-3d visual-studio-code-bin flashplugin python-pipenv otf-fira-code consolas-font powerline-fonts gitkraken

## Configurações do sistema

### Mirrorlist

	$ sudo pacman-mirrors --country Brazil Germany && sudo pacman -Syyu

### fstab
	$ mkdir ~/Arquivos
	$ blkid (para lista de UUIDs)
	$ sudo echo -e "UUID=<uuid>\t/home/badaro/Arquivos\tntfs-3g\tdefaults\t0\t0" >> /etc/fstab
	$ sudo mount -a

### Swapfile

	# fallocate -l 4G /swapfile
	# chmod 600 /swapfile
	# mkswap /swapfile
	# swapon /swapfile

Adcione a linha no arquivos ``/etc/fstab``
	
	/swapfile none swap defaults 0 0

### yaourt

Para ignorar a maior parte das confirmações
	
	$ cp /etc/yaourtrc ~/.yaourtrc
	
descomente e muda as opções

	BUILD_NOCONFIRM=1
	EDITFILES=0

## Configurações de pacotes

### Arduino

	$ sudo gpasswd -a $USER lock && sudo gpasswd -a $USER uucp

### Oh-my-zsh

Habilitar o zsh como shell padrão

	$ chsh -s $(which zsh)

Script de instalação	

	$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	
agnoster é um tema maneiro, precisa de fontes powerline.

### Anaconda

No arquivo ~/.profile faça a seguinte modficação para tornar o anaconda o pacote secundário de python (normalmente o caminho para o ananconda é /opt/anaconda/bin):

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

Configure server from a browser:
	
<localhost:9510/web>

### KiCad
	
Biblioteca: https://github.com/benwis/SparkFun-Kicad-Libraries

### pacman

Excluir pacores orfãos
	
	$ sudo pacman -Rns $(pacman -Qtdq)

Excluir arquivos não usados

	$ sudo pacman -Sc

### Julia

	$ yaourt -Sy julia

Inicialize o ``julia`` e instale o pacote ``IJulia`` para habilita-lo no jupyter:
	
	using Pkg
	Pkg.add("IJulia")

### Redshift

Para impedir que o ``geoclue`` peça permissão para pra executar adcione o seguinte no arquivo ``/etc/geoclue/geoclue.conf``

	[redshift]
	allowed=true
	system=false
	users=