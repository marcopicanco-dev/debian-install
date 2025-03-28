<p align="center">
<img width="800px" src="https://linuxnews.de/wp-content/uploads/2023/08/logo-debian-900x400.png" align="center" alt="white" /><br><br>
 
<!-- (site para ícones: https://shields.io/ ) -->
 
<img alt="Maintained" src="https://img.shields.io/badge/Maintained%3F-Yes-green">
<img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/marcopicanco.dev/debian-post-install">
<img alt="GitHub repo size" src="https://img.shields.io/github/repo-size/marcopicanco.dev/debian-post-install">
<img alt="GitHub commit activity (branch)" src="https://img.shields.io/github/commit-activity/y/marcopicanco.dev/debian-post-install">

</p>


# Descrição
O documento abordará a pós-instalação e configuração do Debian 12 Bookworm para desenvolvimento Ruby on Rails. O processo inclui:
<br>
* Configuração de Usuário e Privilégios – Adição do usuário ao sudoers.
* Instalação de Pacotes Essenciais – Ferramentas de compilação e dependências do Ruby on Rails.
* Configuração do Zsh e Oh My Zsh – Troca do shell padrão para Zsh.
* Gerenciador de Versões (Mise) – Instalação e configuração para gerenciar versões do Ruby e Node.js.s
* Instalação do Ruby on Rails – Configuração do Ruby, Bundler e Rails.
* Configuração do Git e SSH – Personalização do Git e geração de chave SSH.
* Instalação do MongoDB – Adição do repositório e configuração do banco de dados.
* Este guia garantirá um ambiente otimizado para desenvolvimento Ruby on Rails no Debian 12.

</br>

# Objetivos
Carregando...

</br>

# Preparação do Debian 12 Bookworm - Pós-Instalação

## Adicionar privilégios de sudo ao novo usuário (o usuário que você escolheu na instalação)
Discorrer sobre os usuários e seus respectivos privilégios.

```shellscript
su -  (Digite sua senha de root)
nano /etc/sudoers
Adicione o seu usuário abaixo do usuário root

#User privilege specification
root ALL=(ALL:ALL) ALL
```

</br>

## Instalação dos pacotes essenciais para o desenvolvimento


Antes de iniciarmos, vamos desabilitar o repositório do CD/DVD
```shellscript
sudo nano /etc/apt/sources.list

Procure algo como:
deb cdrom:[Debian GNU/Linux 12.4.0 _Bookworm_ - Official amd64 NETINST with firmware 20231210-17:56]/ bookworm main non-free-firmware

Em seguida, comente a linha

# deb cdrom:[Debian GNU/Linux 12.4.0 _Bookworm_ - Official amd64 NETINST with firmware 20231210-17:56]/ bookworm main non-free-firmware

Salve e feche o arquivo.

```
**Instalação dos pacotes essenciais.** 

![Driver NVIDIA no Debian - Guia COMPLETO para instalar e configurar](https://i.ytimg.com/vi/VIXTJYzVzR4/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLBxati6ZRIZlixo-do7db2s9w-q3A)

Após ativar os repositórios extras, basta fazer uma atualização completa do sistema e executar os comandos abaixo.

```shellscript
sudo apt install build-essential zlib1g-dev libssl-dev libreadline-dev libyaml-dev libsqlite3-dev libxml2-dev libxslt1-dev libcurl4-openssl-dev libffi-dev git curl zsh -y
```

</br>


**Particularmente eu prefiro usar o zsh ao bash.**
Então vamos mudar: Vamos instalar o OH MY ZSH

![Driver NVIDIA no Debian - Guia COMPLETO para instalar e configurar](https://ohmyz.sh/img/themes/omz-cloud.png)
```shellscript
vou usar o curl

sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

```
```shellscript
Torne o Zsh o shell padrão

chsh -s $(whish zsh)

Verifique se a mudança ocorreu

echo $SHELL

Isso deve retornar algo como:

/bin/zsh

```

**Gerenciador de versões de linguagem**
Discorrer sobre as preferências e outras opções de gerenciadores:
[Mise ](https://mise.jdx.dev/getting-started.html)


```shellscript
curl https://mise.run | sh

Adicione o Mise ao zshrc, ao final do arquivo
#MISE
export PATH="$PATH:$HOME/.local/bin"

Recarregue o Zsh
source ~/.zshrc

Agora, o Mise deve estar corretamente no seu PATH. Você pode testar executando:
echo $PATH | grep mise
```
**Gerenciador de versões de linguagem**
Discorrer sobre as preferências e outras opções de gerenciadores:

```shellscript
mise doctor

Caso seja detectado 1 problema, digite:

export PATH="$PATH:$HOME/.local/share/mise/shims"

Em seguida:

source ~/.zshrc

E rode novamente o mise doctor.

```
**Instalando o Rails**
[Rails Guide](https://guides.rubyonrails.org/)
```shellscript
mise use -g ruby@3

mise instalar a última versão atualizada.
```
**Instalando  Bundler**
Discorrer sobre as preferências e outras opções de gerenciadores:

```shellscript
gem install bundler

Atualize o RubyGems
gem update --systema

```
**Instale o Rails**
Discorrer sobre as preferências e outras opções de gerenciadores:

```shellscript

Instale o Rails
gem install rails
```
**Instalando  Node**

```shellscript
Instale o node

mise use -g node@22
```

**Configurando o Git e chave SSH**
[GitHub](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```shellscript
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"

Em seguida verifique se deu certo:

git config --global --list

Gerando uma nova chave SSH
ssh-keygen -t ed25519 -C "seu@email.com"

Adicionar o chave pública (configurações/SSH and GPG Keys)

```

**Instalando o MongoDB**

[MongoDB](https://www.mongodb.com/pt-br/docs/manual/tutorial/install-mongodb-on-debian/)
```

1. Importe a chave pública.
A partir de um terminal, instale gnupg e curl, se ainda não estiverem disponíveis:

sudo apt-get install gnupg curl

Para importar a chave GPG pública do MongoDB, execute o seguinte comando:

curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
   --dearmor

2. Crie o arquivo de lista.

echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] http://repo.mongodb.org/apt/debian bookworm/mongodb-org/8.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list

3. Recarregue o banco de dados do pacote .
sudo apt-get update

4. Instale o servidor da MongoDB Community .

sudo apt-get install -y mongodb-org

5. Inicie o Mongo
sudo systemctl start mongod


```
**Ative o Mise pra que possa dectatar automaticamente as versões dos projetos**

```
 echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc
```

**Atualização do bundler update**

```
Warning: the running version of Bundler (2.1.4) is older than the version that created the lockfile (2.4.19). We suggest you to upgrade to the version that created the lockfile by running `gem install bundler:2.4.19`.
```


**Adicione o mise à configuração do seu shell (bashrc ou zshrc)**
Discorrer sobre as preferências e outras opções de gerenciadores:

```shellscript
ASDF
```
```shellscript
Rbenv
```
```shellscript
RVM
```


</br>

# Resultado final com neofetch.
```
Descrição
```
<!-- </br>

Faça o download da versão gratuita do [Davinci Resolve](https://www.blackmagicdesign.com/br/products/davinciresolve) no site oficial da Black Magic, em meu uso diário não tenho enfrentado nenhum problema com o instalador padrão além dos citados neste guia. 

</br>

**Resolução de dependências para o Davinci Resolve**

Em algumas instalações o Davinci Resolve não inicia devido a falta de dependências no sistema, uma das formas de corrigir este problema é instalar os pacotes abaixo no Debian 12.

```shellscript
sudo apt install libxcb-composite0 libxcb-cursor0 libxcb-xinerama0 libxcb-xinput0
```

**OBS.:** Tenho observado alguns bugs na versão 18.6 e posteriores, principalmente relacionado com a gestão das timelines, por isso, recomendo que você faça alguns testes e valide se no seu ambiente está tudo funcionando corretamente. No momento, sigo utilizando a versão 18.5.

</br>

**Contornar erro de instalação "do pacote"** 

Este problema ocorreu comigo apenas na instalação do Davinci Resolve no openSUSE Tumbleweed e em instalações feitas no modo avançado do Debian. Ao executar o instalador, é exibida uma mensagem de que exitem pacotes faltando no sistema e mesmo instalando os pacotes o instalador não inicia.

Deixo aqui anotado caso afete a instalação de outras pessoas.

```shellscript
SKIP_PACKAGE_CHECK=1 ./DaVinci_Resolve_18.X_Linux.run
```

</br>

**Corrigir o erro com instalador gráfico do Resolve "libfuse2"**

Caso o instalador gráfico do Davinci Resolve não abra, execute ele via terminal para ver qual é a mensagem de erro. Caso apareça algo similar a "*libfuse.so.2: cannot open shared object file*" - use o comando abaixo para contornar o problema.

```shellscript
apt install -y libfuse2
```

</br>

**Resolver problemas com libs do Davinci Resolve"**

O pacote do Davinci Resolve incorpora uma série de bibliotecas que podem conflitar com as versões disponíveis em algumas distros Linux. Existem formas diferentes de contornar esta situação caso ocorra com você, nesta página da [Arch Wiki](https://wiki.archlinux.org/title/DaVinci_Resolve) existem diversas dicas que podem ser úteis. 
Em minhas instalações, geralmente apagar as libs abaixo já resolvem o problema do Resolve. Sugiro que você faça um backup dos arquivos antes de removê-los do sistema. :-)


O comando abaixo cria uma cópia dos arquivos das bibliotecas dentro da home do usuário resolvendo links simbólicos.

```shellscript
tar -cvhzf ~/backup-libs-resolve.tar.gz /opt/resolve/libs/libgmodule-2.0.so* /opt/resolve/libs/libglib-2.0.so* /opt/resolve/libs/libgio-2.0.so*
```

Agora é só apagar as bibliotecas que geralmente dão problemas. **Muita atenção ao executar estes comandos, qualquer erro de digitação pode gerar uma quebra severa do sistema.**

```shellscript
sudo rm /opt/resolve/libs/libgmodule-2.0.so*
sudo rm /opt/resolve/libs/libglib-2.0.so*
sudo rm /opt/resolve/libs/libgio-2.0.so*
```

</br></br>

# Preparação do ambiente para produtividade

## Instalação de ferramentas gráficas: Gimp, Inskcape, Shotcut, ColorPicker.

Canivete suíço de criação de conteúdo, tratamento de imagens, desenho vetorial e edição de vídeo usando software livre.

```shellscript
flatpak install org.gimp.GIMP com.obsproject.Studio nl.hjdskes.gcolor3 org.flameshot.Flameshot org.inkscape.Inkscape org.shotcut.Shotcut
```

</br>

## Instalação de navegadores web: Google Chrome, Microsoft Edge, Firefox e Chromium.

Eu deixo os principais navegadores instalados para que possa fazer diversos tipos de testes em sites e aplicativos web. O Firefox e o Chromium instalo as versões do repositório do Debian.

```shellscript
flatpak install com.google.Chrome com.microsoft.Edge
```

</br>

## Instalação de programas diversos: Winff, Video Trimmer, MPV, Timeshift, Boxes, VirtualBox.

Esta sessão é totalmente livre e aqui listo vários programas auxiliares que utilizo diariamente, sugiro fortemente que daqui para baixo, ajuste conforme suas preferências.

```shellscript
flatpak install com.system76.Popsicle md.obsidian.Obsidian org.onlyoffice.desktopeditors org.gnome.gitlab.YaLTeR.VideoTrimmer org.x.Warpinator
```

```shellscript
flatpak install com.usebottles.bottles com.github.tchx84.Flatseal org.gnome.Boxes
```

```shellscript
sudo apt install vim bashtop fish gpm yt-dlp ttf-mscorefonts-installer fonts-bebas-neue chromium aria2
```

Obs.: parei de utilizar o Pika Backup após sofrer 2 corrompimentos seguidos de backup que não puderam ser recuperados.

</br>

# Configurações extras

**Plataformas de Jogos**

Instala os pacotes flatpak necessários para a Steam e Heroic Games Launcher.

```shellscript
flatpak install com.valvesoftware.Steam com.valvesoftware.Steam.Utility.MangoHud com.valvesoftware.Steam.Utility.vkBasalt com.valvesoftware.Steam.VulkanLayer.MangoHud com.heroicgameslauncher.hgl
```
Se for necessário, utilizando o FlatSeal libere as permissões do pacote flatpak do Steam para acessar outras unidades de disco.

</br>

**Extensões do GNOME**

Apesar de não ser incentivado pelo projeto GNOME, ainda utilizo algumas extensões em meu ambiente.
- [AppIndicator and KStatusNotifierItem Support](https://extensions.gnome.org/extension/615/appindicator-support/)
- [GSConnect](https://github.com/GSConnect/gnome-shell-extension-gsconnect/wiki)
- [Blur My Shell](https://github.com/aunetx/blur-my-shell)
- [Screenshot Window Sizer](https://extensions.gnome.org/extension/881/screenshot-window-sizer/)

</br>

**Remoção de pacotes desnecessários**

Limpeza de pacotes que são instalados por padrão e que não utilizo em minha rotina.

```shellscript
sudo apt purge libreoffice-common gnome-games --autoremove
```

</br>

**Tema de ícones**

Nas minhas instalações eu gosto de utilizar o tema para ícones [Papirus](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme) na variante dark, ele existe nos repositórios oficiais. Um dos motivos para utilizar este tema é que ele cobre todos os programas que eu uso, no tema Adwaita padrão, faltam ícones para diversos programas.

Basta instalar o tema e ativar usando o GNOME Ajustes.

```shellscript
sudo apt install papirus-icon-theme
``` -->
