# Configurando Pop_OS no Thinkpad T480

Algumas configurações no Pop_OS 20.10, Gnome 3.38 rodando no X11.

## Power Management
Para uma melhor gestão de energia e economia de bateria instalar o TLP e o powertop.

```
sudo add-apt-repository ppa:linrunner/tlp
sudo apt install tlp tlp-rdw -y
sudo apt install acpi-call-dkms -y
sudo apt install powertop -y
tlp start
```

As configurações padroes do TLP já são suficientes para melhorar o desempenho da bateria. O powertop, no entanto, deve ser calibrado e configurado. Primeiro rodar `sudo powertop -calibrate`. Em seguida, para aplicar as configurações, rodar o `powertop` ir até a ultima aba e aplicar as mudanças manualmente ou então rodar `sudo powertop --auto-tune` para setar todas as opções para GOOD automaticamente. 

As configurações do powertop devem ser reaplicadas a cada nova sessão. Para automatizar esse processor é possível seguir o tutorial na wiki do Arch para criar inicializar o powertop como um serviço.

1. [https://wiki.archlinux.org/index.php/powertop](https://wiki.archlinux.org/index.php/powertop)
2. [https://wiki.archlinux.org/index.php/TLP](https://wiki.archlinux.org/index.php/TLP)

## Turbo Boost e Undervolt

O T480 tem um problema com um firmware da Intel que impede que limita o clock do processador. Para impedir que esse throttle aconteça em condições normais é preciso instalar o `throttled` por [esse repositório](https://github.com/erpalma/throttled). Com esse pacote também é possível fazer o undervolt. No meu T480 tenho deixado com -90mV e o sistema tem se mantido estável.

```
sudo apt install git build-essential python3-dev libdbus-glib-1-dev libgirepository1.0-dev libcairo2-dev python3-venv python3-wheel
git clone https://github.com/erpalma/throttled.git
sudo ./throttled/install.sh
```
 
## Hibernação

1. https://medium.com/@csatyendra02/pop-os-hibernate-enable-step-by-step-complete-tutorial-and-references-601e0ca4c96e
2. https://pop-planet.info/forums/threads/guide-to-hibernate-answer-is-a-guide.426/

## Bug das teclas de Multimidia em teclados ABNT2

Existe um bug no X11 quando as teclas multimidias são usadas em conjunto com um teclado ABNT2. Nesses casos quando as teclas multimidas são pressionadas o uso dá CPU dá um pulo de alguns segundo para 100%. Esse problema e a solução estão relatados no link abaixo. Para evitar esse problema. No Wayland essas teclas funcionam sem problemas.

```
/usr/share/X11/xkb/symbols/br
---
Comment the line: modifier_map Mod3 { Scroll_Lock };
```

1. [https://askubuntu.com/questions/906723/fn-media-keys-slow-delayed-on-ubuntu-gnome-17-04](https://askubuntu.com/questions/906723/fn-media-keys-slow-delayed-on-ubuntu-gnome-17-04)

## libinput-gestures

Para habilitar alguns gestos no trackpad no X11 estou usando o libinput-gestures. Alguns desses gestos já são implementados por padrão no Wayland mas como ainda não é possível compartilhar a tela em chamadas de vídeos no Wayland continuo usando o X. Para instalar o `libinput-gestures` só seguir as instruções em: [https://github.com/bulletmark/libinput-gestures](https://github.com/bulletmark/libinput-gestures)


Minhas Configurações:
```
/etc/libinput-gestures.conf
---
# Move workspaces
gesture swipe up	xdotool key ctrl+super+Down
gesture swipe down	xdotool key ctrl+super+Up

# Browser back/forward
gesture swipe left	3 xdotool key alt+Right
gesture swipe right	3 xdotool key alt+Left
```

## Outros Programas

### Firefox

Tenho usado o Firefox por ele ser o único que implementa algum tipo de kinectic scrolling no Linux. Para habilitar essa configração e a aceleração de hardware é preciso setar algumas variaveis de ambiente.

```
~/.profile
---
export MOZ_USE_XINPUT2=1	# Smooth Scrolling
export MOZ_X11_EGL=1 		# HW Acceleration
```

### DDCUtil

O ddcutil permite configurar o brilho do monitor por softaware. 

1. https://github.com/daitj/gnome-display-brightness-ddcutil/blob/master/README.md
2. https://extensions.gnome.org/extension/2645/brightness-control-using-ddcutil/

### Google-drive-ocaml

...

## Configurações Estéticas

Tenho mudado as fontes padrões e os ícones por não achar os padões do Ubuntu/Pop_OS agradáveis. Como fonte sans-serif do sistema tenho usado a [Inter](https://rsms.me/inter/). Como fonte monospace tenho usado a [firacode](https://github.com/tonsky/FiraCode). Depois de instalada a Firacode ainda é necessário mudar algumas configurações para os editores de texto reconhecerem essa fonte como monospace.

```
sudo apt install fonts-inter -y
sudo apt install fonts-firacode -y
sudo apt install fonts-ebgaramond -y
sudo apt purge fonts-noto* fonts-noto-cjk* fonts-noto-cjk-extra* fonts-noto-core* fonts-noto-extra* fonts-noto-mono* fonts-noto-ui-core* fonts-noto-ui-extra* fonts-noto-unhinted*
```

1. Como tema do GTK tenho usado o **Nextwaita**: [https://www.gnome-look.org/p/1289376/](https://www.gnome-look.org/p/1289376/)
2. Como tema do Gnome tenho usado o **AdwaitaEx** por ter uma barra de status mais compacta: [https://github.com/hrdwrrsk/AdwaitaExtended](https://github.com/hrdwrrsk/AdwaitaExtended)
3. Como ícones tenho usado o **Elementosh-Blueberry**: [https://www.gnome-look.org/p/1427890/](https://www.gnome-look.org/p/1427890/)