---
title: "Guia iniciante no docker"
description: "Inicie o aprendizado de docker: nesse artigo eu passo pelos conceitos fundamentais do docker como imagem e container"
date: 2026-07-01
draft: false

categories:
 - docker
 - linux
 - containers

tags:
  - docker
  - containers
  
---


O **Docker** é uma plataforma de código aberto que permite empacotar e executar aplicativos em ambientes isolados chamados **contêineres**.

O Docker ficou conhecido por ajudar a resolver o clássico problema: "na minha máquina funciona", pois empacota a aplicação junto com suas dependências, tornando o ambiente de execução muito mais previsível.

- **O que são contêineres**: Um contêiner é uma instância em execução de uma imagem Docker. Ele executa um ou mais processos isolados e leva consigo tudo o que a aplicação precisa para funcionar, como bibliotecas, dependências e configurações.

- **Imagem Docker**: Uma imagem Docker é um modelo somente leitura que contém tudo o que é necessário para criar um contêiner, Pense nela como uma receita de bolo que descreve como o contêiner deve ser criado, mas não está em execução. o contêiner é a imagem em execução.

Uma imagem é imutável (somente leitura),
Quando um contêiner é criado o Docker adiciona uma camada gravável sobre a imagem, Dessa forma a imagem original permanece inalterada enquanto todas as modificações ficam armazenadas apenas naquele contêiner.

é possível criar vários conteires tendo a mesma imagem como base, e cada container é isolado um do outro e do sistema base.

 Os contêineres utilizam diretamente o kernel do sistema Linux hospedeiro, Eles não possuem um sistema operacional próprio nem utilizam um hypervisor, como VirtualBox ou VMware para intermediar o acesso ao hardware.

 O Docker utiliza recursos do kernel Linux, como Namespaces (isolamento) e Cgroups (controle de recursos), para limitar e isolar os contêineres e todos os contêineres compartilham o mesmo kernel do sistema hospedeiro.

## Instalação

### Windows

Para o docker rodar em sistemas windows ele cria mini maquinas virtuais(usando WSL2) já que ele depende do kernel Linux.

para a instalação no windows é fundamental ter o WSL2 instalado e ativado então vale a pena dar uma olhada no site da microsoft sobre o assunto: [Como instalar o Linux no Windows com o WSL](https://learn.microsoft.com/pt-br/windows/wsl/install).

depois disso é só baixar o instalador: [Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/
)

na instalação marque a caixa para usar o wsl2 e reinicie o computador, apos a instalação talves peça mais alguns passos extras depois disso om docker está instalado.

nesse artigo eu vou focar mais em sistemas que usam o kernel Linux mas nada impede que quem estiver lendo isso entenda.

### Linux

O docker funciona nativamente no Linux então a instalação é mais tranquila.
aqui eu vou instalar apenas o Docker Engine que é o suficiente para demonstrar os comandos.

baixando:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```

instalando:

```bash
sudo sh get-docker.sh
```

adicione seu usuário no grupo do docker:

```bash
sudo usermod -aG docker $USER
```

Depois de fazer login novamente (ou reiniciar a sessão), o Docker poderá ser utilizado sem precisar executar os comandos com sudo.

## primeiros comandos

Verificando versão do docker:

```bash
docker --version
```

criando seu primeiro container de exemplo:

```bash
docker run hello-world
```

esse container de teste contem apenas um programinha escrito  linguagem **c** que imprime uma mensagem no terminal.

basicamente esse comando está pedindo para rodar o container hello-world mas como a imagem não existe localmente o docker vai buscar ela no Docker Hub, logo depois de baixar a imagem ele cria o container e executa o programa exibindo a mensagem no terminal e fechando logo em seguida.

é importante saber que Um container vive enquanto o processo principal (PID 1) estiver em execução, como o programa é  executado e logo depois fecha o container também se encerrar.

Muito bem!  Você acabou de executar seu primeiro contêiner Docker, Neste artigo você aprendeu os conceitos fundamentais e preparou seu ambiente, Nos próximos artigos vamos conhecer os comandos mais importantes do Docker e aprender a gerenciar contêineres e imagens.
