---
title: "Guia completo do Distrobox no Debian"
date: 2026-06-27
draft: false

categories:
  - linux

tags:
  - debian
  - distrobox
  - podman
  - containers
---

![Instalação Distrobox](/images/posts/captura-distrobox.png)

O **Distrobox** é uma ferramenta que permite criar ambientes isolados (containers) utilizando diferentes distribuições Linux, como Ubuntu, Fedora, Arch Linux, Debian, openSUSE e muitas outras, sem precisar instalar máquinas virtuais.

Diferente de um container tradicional, o Distrobox integra o ambiente com o sistema hospedeiro, compartilhando sua pasta pessoal, dispositivos, tema, variáveis de ambiente e até permitindo exportar aplicações gráficas para o menu do sistema.

Isso o torna uma excelente ferramenta para desenvolvimento, testes e aprendizado.

Neste guia será utilizado o **Debian** como sistema hospedeiro (host).

---

# Instalação

## 1. Atualize o sistema

Antes de instalar qualquer pacote, atualize a lista de repositórios e os pacotes instalados.

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. Instale o Podman

O Distrobox utiliza um runtime de containers. Neste guia será utilizado o **Podman**, que já está disponível nos repositórios oficiais do Debian.

```bash
sudo apt install -y podman
```

---

## 3. Verifique a configuração Rootless

Para executar containers sem utilizar `sudo`, o Podman utiliza o mecanismo de **subUID** e **subGID**.

Na maioria das instalações do Debian essa configuração já existe, mas vale conferir:

```bash
cat /etc/subuid /etc/subgid
```

A saída deverá conter uma entrada semelhante a:

```text
seu_usuario:100000:65536
```

Caso os arquivos estejam vazios, será necessário configurá-los antes de utilizar o Distrobox.

---

## 4. Instale o Distrobox

O Distrobox também está disponível nos repositórios oficiais do Debian.

```bash
sudo apt install -y distrobox
```

Verifique se tudo foi instalado corretamente:

```bash
distrobox --version
```

---

# Criando um container

A sintaxe geral é:

```bash
distrobox create --name <nome> --image <imagem:tag>
```

## Ubuntu

```bash
distrobox create --name ubuntu --image ubuntu:latest
```

ou uma versão específica:

```bash
distrobox create --name ubuntu --image ubuntu:26.04
```

---

## Debian

```bash
distrobox create --name debian --image debian:stable
```

---

## Fedora

```bash
distrobox create --name fedora --image fedora:latest
```

---

## Arch Linux

```bash
distrobox create --name arch --image archlinux:latest
```

---

Na primeira execução o Distrobox perguntará se deseja baixar a imagem.

Responda:

```text
y
```

Depois aguarde o download e a configuração inicial do ambiente.

---

# Entrando no container

Para acessar um container:

Ubuntu

```bash
distrobox enter ubuntu
```

Fedora

```bash
distrobox enter fedora
```

Arch

```bash
distrobox enter arch
```

Na primeira inicialização o Distrobox fará algumas configurações automaticamente, como compartilhamento da pasta pessoal, integração com o tema do sistema e variáveis de ambiente.

Esse processo acontece apenas uma vez.

---

# Instalando programas

Depois de entrar no container, utilize o gerenciador de pacotes da distribuição normalmente.

Ubuntu / Debian

```bash
sudo apt update
sudo apt install python3 python3-pip
```

Fedora

```bash
sudo dnf install python3 python3-pip
```

Arch Linux

```bash
sudo pacman -Syu python python-pip
```

---

# Saindo do container

Para sair basta executar:

```bash
exit
```

---

# Comandos úteis

Entrar em um container

```bash
distrobox enter ubuntu
```

Listar containers

```bash
distrobox list
```

Parar um container

```bash
distrobox stop ubuntu
```

Remover um container

```bash
distrobox rm ubuntu
```

Atualizar os containers

```bash
distrobox upgrade
```

---

# Observações

- Os containers compartilham sua pasta **HOME** com o sistema hospedeiro.

- Cada container possui seu próprio gerenciador de pacotes.

- Você pode criar quantos containers desejar.

- O nome do container não precisa ser igual ao nome da distribuição.

- A opção `latest` sempre baixa a versão mais recente disponível da imagem.

Como o Distrobox utiliza imagens OCI é possível usar praticamente qualquer distribuição Linux que possua uma imagem oficial disponível.

Alguns exemplos:

- Ubuntu

- Debian

- Fedora

- Arch Linux

- openSUSE

- AlmaLinux

- Rocky Linux

- Kali Linux

- Alpine Linux

- Gentoo
