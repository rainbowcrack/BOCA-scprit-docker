# Passo a Passo de instalação do BOCA para Docker - via serviços em Nuvem

Nessa demonstração, estou usando preferencialmente o **Google Shell**

1. Use o comando para subir o docker e uma imagem linux dentro dele, com portas alternativas:
```bash
docker run -dit \
--name boca \
-p 6081:6080 \
ubuntu:22.04
````

2. Entre como root no docker:
```bash
docker exec -it boca bash
```
3. Atualize o seu docker:
```bash
apt update
```
4. Faça a instalação geral:
```bash
apt install -y \
sudo \
nano \
wget \
curl \
xfce4 \
xfce4-goodies \
xfce4-terminal \
tightvncserver \
dbus-x11 \
xterm \
novnc \
websockify \
net-tools
```
OBS.: conferir de o time-zone e sazonal é 136 (São Paulo), além do teclado 77 ou 76 Portuguese-Brazil ou outro de sua preferência de país.

5. Crie a pasta VNC e inicie o VNC
```bash
mkdir -p ~/.vnc
vncserver :1
```
6. Se der problemas de senha, mate a sessão
```bash
$export USER=root$
vncserver -kill :1
nano /root/.vnc/xstartup
```
Apague todo o conteúdo de xstartup e escreva:

```bash
#!/bin/bash

unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS

export XDG_RUNTIME_DIR=/tmp/runtime-root
mkdir -p $XDG_RUNTIME_DIR
chmod 700 $XDG_RUNTIME_DIR

exec dbus-launch --exit-with-session startxfce4
```
* CTRL+O
* ENTER
* CTRL+X

Dê permissão de execução:
```bash
chmod +x /root/.vnc/xstartup
```

Reinicie o vnc:
```bash
vncserver :1 -geometry 1366x768 -depth 24
```

Configure o vnc:
```bash
ln -sf /usr/share/novnc/vnc.html /usr/share/novnc/index.html
```

Websockify:
```bash
websockify --web=/usr/share/novnc/ 6080 localhost:5901
```
7. Inicie a Websockify
```bash
websockify --web=/usr/share/novnc/ 6080 localhost:5901
```
MAPEAMENTO: 6081 -> 6080
## Se der conflitos

ERROS:
Mate:
```bash
vncserver -kill :1 || true
````
Limpe logs:
```bash
rm -rf /tmp/.X11-unix/X1
```
Crie váriaveis locais:
```bash
export USER=root
export HOME=/root
```
Se der conflito de docker, remova a imagem antiga ou crie outro:
```bash
docker rm -f boca
```