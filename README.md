# Homelab - Ubuntu server e Docker

Estou criando esse repositorio para documentar o meu processo de instalação de um Ubuntu server e do Docker. Acabei de tirar a LPI Linux Essentials e quero praticar tudo que aprendi e também treinar com Docker.

---
# Instalação e Hardware

- **ISO:** Ubuntu Server 24.04 LTS
- **Hardware:** Notebook Dell Inspiron 15
    **Processador:** Intel Pentium Gold 7505 - Dual-Core com clock 2 GHz a 3.5 GHz
    **Memória RAM:** 4 GB DDR4
    **Armazenamento:** HD de 512 GB
- **Rede:** IP estático 192.168.1.100, interface wl0n

---

# Pós-instalação imediata

#### Atualização de sistema 
```bash
sudo apt update && sudo apt upgrade -y
```

#### NetworkManager
```bash
sudo apt install network-manager -y
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```
Pretendo utilizar com Wifi e realizar as configurações de redes através do nmtui eu acredito ser mais fácil.

#### Instalação de programas necessários, como: git, curl e vim.
```bash
sudo apt install git curl vim htop -y
```

#### Configurar SSH
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw allow 22
sudo ufw status
```
Habilitamos para iniciar com o sistema, e fizemos a permissão no Firewall.
Vou utilizar a porta 22 por padrão, mas o correto é trocar por um diferente entre 1024 e 65535.

---
# Instalação Docker

#### Remover versões antigas
```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

#### Adicionar o repositório e chave GPG no sistema
```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```
Toda vez que dermos update e upgrade irá atualizar automaticamente o Docker também.

#### Startar com o sistema
```bash
sudo systemctl enable --now docker
```

#### Adicionar usuario no grupo 'docker'
Para rodarmos com nosso usuario temos que adiciona-lo antes no grupo 'docker'.
```bash
sudo usermod -aG docker $USER
newgrp docker
```

---
## Problemas encontrados:
- Teclado com layout incorreto

Como resolver:
```bash
sudo dpkg-reconfigure keyboard-configuration
#Escolher o modelo e linguagem correto#

sudo systemctl restart keyboard-setup
#Reiniciar serviço
```

