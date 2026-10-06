# Prática 1 — Flask, Redis e Docker

Este projeto mostra uma página Flask com um contador de acessos. O número é guardado no Redis, então continua disponível mesmo quando os containers são recriados, desde que o volume seja mantido.

```text
Navegador → Flask → Redis → Volume
```

## O que há no projeto

- `app.py`: aplicação Flask e página do contador.
- `Dockerfile`: instruções para criar a imagem da aplicação.
- `docker-compose.yml`: configuração da aplicação, do Redis, da rede e do volume.
- `RESPOSTAS.md`: respostas às perguntas da atividade.
- `Vídeo_Funcionamento_Sistema.mkv`: demonstração do funcionamento.

## Pré-requisito

Instale e inicie o Docker Desktop. Abra o terminal na pasta do projeto, onde estão `app.py` e `docker-compose.yml`.

## Executar com Docker Compose

Construa a imagem e inicie a aplicação e o Redis:

```powershell
docker compose up -d --build
```

Abra [http://localhost:5050](http://localhost:5050). Atualize a página para ver o contador aumentar.

Para conferir os containers, a rede e o volume:

```powershell
docker compose ps
docker network ls
docker volume ls
```

## Testar se o contador é mantido

1. Acesse a página algumas vezes e observe o número.
2. Pare e remova os containers:

   ```powershell
   docker compose down
   ```

3. Inicie-os novamente:

   ```powershell
   docker compose up -d
   ```

4. Acesse a página. O contador continua de onde parou, pois o Redis usa um volume.

Não use `docker compose down -v` nesse teste: a opção `-v` apaga também o volume e os dados salvos.

## Alterar a mensagem da página

Edite o título em `app.py`, salve o arquivo e reconstrua a aplicação:

```powershell
docker compose up -d --build
```

Atualize a página no navegador para ver a alteração. O `--build` é necessário para incluir o código editado na imagem.

## Execução manual dos containers

Também é possível iniciar cada container sem Compose. Execute estes comandos na pasta do projeto:

```powershell
docker build -t app-flask .
docker network create app-rede
docker volume create redis-dados
docker run -d --name redis --network app-rede -v redis-dados:/data redis:7-alpine redis-server --appendonly yes
docker run -d --name app --network app-rede -p 5050:5050 -e REDIS_HOST=redis app-flask
```

Abra [http://localhost:5050](http://localhost:5050). O nome `redis` permite que a aplicação encontre o Redis pela rede Docker.

Antes de iniciar pelo Compose, remova os containers e a rede da execução manual:

```powershell
docker rm -f app redis
docker network rm app-rede
```

O volume `redis-dados` é mantido. Para encerrar a execução com Compose e manter seus dados:

```powershell
docker compose down
```
