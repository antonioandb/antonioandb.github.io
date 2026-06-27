---
title: "Guia de instalação do Pyenv em distros base Debian/Ubuntu"
description: "Pare de brigar com versões do Python — aprenda a usar Pyenv no Debian/Ubuntu de forma simples"
date: 2026-06-27
draft: false

categories:
  - linux
  - desenvolvimento
  - python
  - pyenv

tags:
  - debian
  - ubuntu
  - python
  - pyenv
  - desenvolvimento
---



O **pyenv** é uma ferramenta de linha de comando que permite instalar, gerenciar e alternar facilmente entre múltiplas versões do Python na mesma máquina, sem interferir no Python do sistema.

---

## 1. Instalação das dependências

Antes de instalar o pyenv, precisamos instalar alguns pacotes essenciais para compilar o Python do zero:

```bash
sudo apt update

sudo apt install -y \
  build-essential curl git \
  libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev \
  libsqlite3-dev wget llvm \
  libncursesw5-dev xz-utils tk-dev \
  libxml2-dev libxmlsec1-dev \
  libffi-dev liblzma-dev ca-certificates
```

Esses pacotes são necessários porque o pyenv compila versões do Python localmente no seu sistema.

---

## 2. Instalando o pyenv

A forma mais simples de instalar o pyenv é usando o instalador oficial:

```bash
curl https://pyenv.run | bash
```

Repositório oficial:  
https://github.com/pyenv/pyenv

---

## 3. Configurando o shell (Bash)

Adicione as variáveis de ambiente no seu `~/.bashrc`:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
```

Depois recarregue o shell:

```bash
source ~/.bashrc
```

---

### Se você usa Zsh

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc

source ~/.zshrc
```

---

## 4. Verificando a instalação

Para confirmar que tudo funcionou:

```bash
pyenv --version
```

Se o comando retornar uma versão, o pyenv está instalado corretamente.

---

## 5. Instalando versões do Python

Para listar todas as versões disponíveis:

```bash
pyenv install --list
```

Para instalar uma versão específica:

```bash
pyenv install 3.13.1
```

---

## 6. Definindo a versão do Python

### Global (sistema inteiro do usuário)

```bash
pyenv global 3.12.3
```

### Local (apenas para um projeto)

Dentro da pasta do projeto:

```bash
pyenv local 3.12.3
```

---

### Mantendo o Python do sistema como padrão

Se você prefere manter o Python do sistema como padrão e usar o pyenv apenas quando necessário:

```bash
pyenv global system
```

---

## 7. Verificando versões instaladas

```bash
pyenv versions
```

Isso lista todas as versões instaladas e indica qual está ativa no momento.

---

## 8. Comandos úteis

### Ver qual Python está sendo usado

```bash
which python
```

---

### Atualizar o pyenv

```bash
cd ~/.pyenv
git pull
```

---

### Remover uma versão instalada

```bash
pyenv uninstall 3.11.9
```

---

##