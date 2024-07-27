# Documentação do VoteStream

## Introdução

O VoteStream é uma API de votação que permite a criação de enquetes e a participação dos usuários por meio de votos. Este documento fornece uma visão geral sobre como configurar e utilizar o VoteStream.

## Requisitos

Antes de começar, verifique se os seguintes softwares estão instalados em seu sistema:

- Node.js (versão 18 ou superior)
- Docker
- Docker Compose

## Instalação

Siga as etapas abaixo para instalar e configurar o VoteStream:

1. Clone o repositório do VoteStream em sua máquina local

2. Crie um arquivo `.env` na raiz do diretório do projeto e defina as variáveis de ambiente necessárias:
```shell
DATABASE_URL="postgresql://docker:docker@localhost:5432/polls?schema=public"
```

3. Inicie os contêineres Docker:

```shell
docker-compose up -d
```

4. Instale as dependências do projeto:

```shell
npm install
```

5. Execute as migrações do banco de dados:

```shell
npx prisma migrate dev
```

6. Inicie o servidor:

```shell
npm run dev
```

## Uso
Exemplos de Requisições

### 1. Criar uma Enquete

Para criar uma enquete, você deve fornecer um título e uma lista de opções. A requisição deve ser no seguinte formato:
- Método: POST
- Endpoint: http://localhost:3333/polls
Corpo da Requisição:

 ```json
{
    "title": "Qual é a sua cor favorita?",
    "options": ["Azul", "Verde",  "Vermelho"]
}
 ```

Resposta:

A criação da enquete retornará um ID único que deve ser utilizado para as requisições subsequentes.

 ```json
{
    "pollId": "d4d76c8d-9abe-46e6-860f-0404b4c28d72"
}
 ```

### 2. Obter uma Enquete

Utilize o ID retornado ao criar a enquete para fazer a requisição GET. Isso retornará o ID das opções, que será necessário para votar:
- Método: GET
- Endpoint: http://localhost:3333/polls/{pollId}
  
Substitua {pollId} pelo ID obtido na criação da enquete.

Resposta:
```json
{
  "poll": {
    "id": "d4d76c8d-9abe-46e6-860f-0404b4c28d72",
    "title": "Qual é a sua linguagem de programação favorita?",
    "options": [
      {
        "id": "8b736c47-1155-4aae-90ec-2388f311f392",
        "title": "JavaScript",
        "score": 1
      },
      {
        "id": "28ae354d-d33b-4807-bbd3-3a8e6423b6a9",
        "title": "Python",
        "score": 0
      },
      {
        "id": "f68fc8cd-2cb4-4795-90e9-0bc603258a22",
        "title": "C#",
        "score": 0
      },
      {
        "id": "c569da22-35ec-4441-8204-1b2e148d74ee",
        "title": "Java",
        "score": 0
      }
    ]
  }
}
```

### 3. Votar em uma Enquete

Para votar, primeiro obtenha o ID da enquete e o ID da opção desejada a partir da resposta da requisição GET. Em seguida, faça a requisição POST:
Substitua `{pollId}` pelo ID da enquete e `{pollOptionId}` pelo ID da opção que deseja votar.
- Método: POST
- Endpoint: `http://localhost:3333/polls/{pollId}/votes`
Corpo da Requisição:

```json
{
    "pollOptionId": "8b736c47-1155-4aae-90ec-2388f311f392"
}
```

## Dados Técnicos

### Estrutura do Banco de Dados

O esquema do banco de dados pode ser visualizado utilizando o Prisma Studio. Para iniciar o Prisma Studio, execute o seguinte comando no terminal:

npx prisma studio

O Prisma Studio estará disponível em: [http://localhost:5555](http://localhost:5555)

### Arquivos de Configuração

- `.env` - Configurações do ambiente
- `docker-compose.yml` - Configurações dos contêineres Docker

## Tecnologias Utilizadas

- *Backend*: Fastify com TypeScript
- *Banco de Dados*: PostgreSQL
- *Cache*: Redis
- *ORM*: Prisma
- *Comunicação em Tempo Real*: WebSockets
