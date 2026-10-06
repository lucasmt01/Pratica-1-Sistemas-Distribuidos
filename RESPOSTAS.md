# Respostas da atividade

## Questão 1

**Os serviços precisam se comunicar. Como fazer isso?**

Colocamos a aplicação e o Redis na mesma rede Docker. Assim, um consegue se comunicar com o outro.

## Questão 2

**Por que utilizar o nome do serviço é mais adequado do que utilizar diretamente o IP do container?**

O IP pode mudar quando o container é recriado. O nome do serviço continua o mesmo, e o Docker o encontra na rede.

## Questão 3

**Por que foi necessário utilizar `--build` depois de alterar a aplicação?**

Porque o código é copiado para a imagem quando ela é construída. O `--build` cria uma imagem atualizada com a alteração.

## Questão 4

**Qual é a função do Dockerfile na aplicação?**

O Dockerfile contém as instruções para criar a imagem: escolhe a base Python, copia os arquivos, instala as dependências e inicia a aplicação.

## Questão 5

**Qual é a função do Redis nessa arquitetura?**

O Redis guarda o contador de acessos. A aplicação aumenta o número cada vez que alguém abre a página.

## Questão 6

**Como o container Flask consegue encontrar o Redis sem conhecer seu endereço IP?**

Os dois containers estão na mesma rede Docker. A aplicação usa o nome `redis`, e o Docker o direciona ao container correto.

## Questão 7

**Qual é a função do volume utilizado pelo Redis?**

O volume guarda os dados do Redis mesmo quando o container é removido. Ao criar outro container usando o mesmo volume, o contador continua salvo.

## Questão 8

**Qual problema o Docker Compose resolve em relação à execução manual dos containers?**

O Compose reúne a configuração da aplicação e do Redis em um arquivo. Com um comando, inicia os serviços e configura a comunicação entre eles.
