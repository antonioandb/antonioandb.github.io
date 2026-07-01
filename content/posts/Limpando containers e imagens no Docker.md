---
title: "Limpando containers e imagens no Docker"
description: "Aprenda a remover containers, imagens e liberar espaço em disco usando os principais comandos do Docker."
date: 2026-07-01
draft: false

categories:

- docker
- linux
- containers

tags:

- docker
- containers
- docker rm
- docker rmi
- docker prune
- docker system prune

---

esse artigo é uma continuação de "Primeiros comandos no docker".

é importante saber como apagar containers que não serão mais usados pois mesmo parados eles ainda ocupam espaço no disco.

o ideal é remover um container que esteja parado, verifique os estados dos containers:

```bash
docker ps -a
```

para o container que voce quer remover, pode usar o nome ou o ID do container:

```bash
docker stop <ID_ou_Nome_do_Container>
```

Depois que o container estiver parado, você pode removê-lo:

```bash
docker rm <ID_ou_Nome_do_Container>
```

- rm = comando herdado diretamente do Linux significa literalmente remover.

Se quiser parar e apagar o container de uma vez só, use o comando com -f (force):

```bash
docker rm -f <ID_ou_Nome_do_Container>
```

- -f = forçar.

mas é bom saber que quando usamos o comando `docker stop` o docker envia um sinal amigável para o container, dizendo que ele precisa fechar os arquivos e salvar.

O docker `rm -f` ignora esse processo, encerra o container imediatamente e remove ele da máquina.

o `docker rm -f` é um comando muito usado pela praticidade, Mas em servidores de produção o correto é sempre `docker stop <nome>` e depois `docker rm <nome>`.

## apagando vários containers

se voce quer apagar vários containers de uma vez existe o comando `prune` ele apaga todos os containers que estão parados.

```bash
docker container prune
```

(O Docker vai te pedir uma confirmação y/n antes de apagar).

mas se voce quiser apagar absolutamente TODOS os containers (rodando ou não):

```bash
docker rm -f $(docker ps -aq)
```

- -a = mostra todos os containers (inclusive os parados)

- -q =  exibe apenas seus IDs.

- `rm -f` =  força a exclusão de todos eles.

também existe a opção --rm, que cria um container temporário, ao criar um contêiner desse jeito ele será apagado quando der um `docker stop`.

```bash
docker run --rm <nome_da_imagem>
```

## apagando imagens

imagens também ocupam espaço em disco,
a remoção de imagens também é muito simples mas com uma observação, só é possível remover uma imagem se não existir nenhum container usando ela, mesmo que o container esteja parado o docker não vai permitir a remoção da imagem.

o comando para remover uma image é muito parecido com o de remover containers `rmi` com a adição do 'i' de imagem.

```bash
docker rmi <ID_ou_Nome_da_Imagem>
```

O fluxo correto é: primeiro remova os containers e depois a imagem, Mas se por algum motivo você realmente precisar remover uma imagem que ainda está sendo usada por algum container utilize:

```bash
 docker rmi -f <ID_da_Imagem>
```

 Qualquer container criado a partir dessa imagem continuará existindo, mas não poderá ser iniciado normalmente, pois a imagem necessária para sua execução foi removida.

o `prune` também funciona com imagens, Ele faz uma varredura segura e apaga apenas as imagens sem uso, sem mexer em nada que esteja rodando ou que seus containers parados precisem.

```bash
docker image prune
```

organizar e saber a hora certa de apagar as imagens é importante, seja para liberar espaço ou para apagar versões antigas e desatualizadas, mas muitas vezes é preciso cuidado.

existe um comando de limpeza que cuid disso:

```bash
docker system prune
```

o que ele faz ? :

Ele vai deletar de uma vez só:

- Todos os containers parados.

- cache de build

- redes não utilizadas

- Todas as imagens sem nome.

Importante: o `docker system prune` não apaga os volumes por padrão Isso evita a perda de dados, Se quiser removê-los também utilize --volumes.
"*Falaremos sobre volumes mais adiante.*"

## Resumo

Os comandos mais usados para limpeza são:

- `docker rm <container>`
  Remove um container parado.

- `docker rm -f <container>`
  Força a parada e remove o container.

- `docker container prune`
  Remove todos os containers parados.

- `docker rmi <imagem>`
  Remove uma imagem.

- `docker image prune`
  Remove imagens sem uso.

- `docker system prune`
  Faz uma limpeza geral do Docker.
