---
title: " Guia completo: instalando o driver NVIDIA no Debian 13"
description: " Guia de instalação correta dos drivers NVIDIA no Debian."
date: 2026-06-27
draft: false

categories:
  - linux

tags:
  - debian
  - debian 13
  - trixie
  - nvidia
  - dkms
  - driver
  - kernel
---

 ![Instalação do driver NVIDIA no Debian 13](/images/posts/captura-nvidia.png)

O Debian é conhecido por sua estabilidade e por seguir uma filosofia de priorizar software livre, Por esse motivo os drivers proprietários da NVIDIA não são instalados automaticamente. 

Mas o próprio projeto Debian disponibiliza esses pacotes em seus repositórios oficiais, tornando a instalação simples e segura.

Neste guia, mostrarei o procedimento que utilizei no Debian 13 (Trixie), incluindo algumas observações importantes sobre o funcionamento do DKMS.

---

## 1. Instale o Debian normalmente

Durante a instalação, habilite os seguintes repositórios:

- `main`

- `contrib`

- `non-free`

- `non-free-firmware`

Caso essa opção não apareça durante a instalação, você poderá configurá-la posteriormente.

---

## 2. Verifique o `sources.list`

No Debian 13 (Trixie), o arquivo deve conter algo semelhante a isto:

```bash
sudo nano /etc/apt/sources.list
```

```text
deb http://deb.debian.org/debian trixie main contrib non-free non-free-firmware
deb http://deb.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
deb http://deb.debian.org/debian trixie-updates main contrib non-free non-free-firmware
```

Atualize a lista de pacotes:

```bash
sudo apt update
```

---

## 3. Atualize completamente o sistema

Antes de instalar o driver da NVIDIA, atualize todos os pacotes:

```bash
sudo apt full-upgrade
sudo reboot
```

Isso evita instalar o driver sobre um sistema parcialmente atualizado.

---

## 4. Instale os pacotes essenciais

```bash
sudo apt install build-essential dkms
```

Esses pacotes serão utilizados para compilar automaticamente o módulo da NVIDIA sempre que um novo kernel for instalado.

---

## 5. Instale os meta-pacotes do kernel

**Este é um dos passos mais importantes.**

```bash
sudo apt install linux-image-amd64 linux-headers-amd64
```

Os meta-pacotes garantem que futuras atualizações instalem automaticamente o novo kernel e seus respectivos headers, permitindo que o DKMS recompile o driver da NVIDIA.

---

## 6. Instale o driver NVIDIA

```bash
sudo apt install nvidia-driver
```

O Debian instalará automaticamente todas as dependências necessárias, incluindo o suporte ao DKMS.

---

## 7. Reinicie o computador

```bash
sudo reboot
```

---

## 8. Verifique se o driver foi carregado

Confira se tudo está funcionando corretamente:

```bash
nvidia-smi
```

```bash
lsmod | grep nvidia
```

```bash
glxinfo | grep renderer
```

Caso o comando `glxinfo` não exista:

```bash
sudo apt install mesa-utils
```

---

# ⚠️ Observação importante sobre o DKMS

Após a instalação, é uma boa ideia verificar se o DKMS registrou corretamente o módulo da NVIDIA.

```bash
dkms status
```

O resultado esperado é algo semelhante a:

```text
nvidia/xxx.xx, 6.x.x-amd64, x86_64: installed
```

### Se aparecer **"dkms: comando não encontrado"**

Foi exatamente o que aconteceu comigo.

Apesar de o pacote `dkms` estar instalado o executável estava localizado em `/usr/sbin`, diretório que não fazia parte da variável `PATH` do meu usuário.

Ao executar:

```bash
/usr/sbin/dkms status
```

o comando funcionou normalmente confirmando que o DKMS estava instalado e que o módulo da NVIDIA havia sido compilado corretamente para o kernel em uso.

Para evitar digitar o caminho completo todas as vezes, adicionei `/usr/sbin` ao meu `PATH`:

```bash
echo 'export PATH=$PATH:/usr/sbin' >> ~/.bashrc
source ~/.bashrc
```

Também conferi a configuração utilizada pelo `sudo`, *mas muito cuidado ao mexer aqui*:

```bash
sudo visudo
```

Na linha `Defaults secure_path`, verifique se `/usr/sbin` está presente.

Abra o `visudo` apenas para verificar a configuração. 

Se `/usr/sbin` já estiver na linha `Defaults secure_path`, feche o editor sem alterar nada.

> **Por que isso é importante?**
> 
> O DKMS é responsável por recompilar automaticamente o módulo proprietário da NVIDIA sempre que um novo kernel é instalado. 
> 
> Se ele não estiver funcionando corretamente o sistema pode inicializar sem o driver de vídeo após uma atualização, resultando em baixa resolução falha ao carregar o ambiente gráfico ou até mesmo uma tela preta. 
> 
> Por isso, vale a pena confirmar que o DKMS está funcionando antes da próxima atualização do kernel.

---

## 10. Após cada atualização de kernel

Antes de reiniciar o computador, execute:

```bash
dkms status
```

Se o módulo da NVIDIA aparecer compilado para o novo kernel, está tudo certo.

Também é possível conferir se os headers da nova versão foram instalados:

```bash
dpkg -l | grep linux-headers
```

Com essas duas verificações, você reduz bastante as chances de ter problemas após atualizar o kernel.