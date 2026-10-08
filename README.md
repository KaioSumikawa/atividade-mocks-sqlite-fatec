# Projeto Monorepo FATEC

## Teste Skip ci

![Status do CI](https://github.com/KaioSumikawa/atividade-mocks-sqlite-fatec/actions/workflows/ci.yml/badge.svg)

## Como rodar o projeto

### Rodando com pnpm (local)

```bash
cd backend
pnpm install
pnpm dev
```

### Rodando com Docker

```bash
docker-compose up --build
```

### Rodando as migrations do banco de dados

```bash
cd backend
pnpm sequelize-cli db:migrate
```

## Scripts disponíveis

### Raiz do monorepo (`package.json`)

| Comando                 | Descrição                         |
| ----------------------- | --------------------------------- |
| `pnpm docker:build-api` | Builda a imagem Docker do backend |
| `pnpm docker:build-app` | Builda a imagem Docker do app     |
| `pnpm compose:up`       | Sobe os containers em background  |
| `pnpm compose:down`     | Derruba os containers             |
| `pnpm compose:logs`     | Acompanha os logs dos containers  |

### Backend (`backend/package.json`)

| Comando      | Descrição                                       |
| ------------ | ----------------------------------------------- |
| `pnpm dev`   | Sobe o servidor em modo desenvolvimento (watch) |
| `pnpm build` | Compila o TypeScript para `dist/`               |
| `pnpm start` | Roda o servidor já compilado (`dist/server.js`) |

## Testes de integração e CI

O backend utiliza SQLite em memória (`:memory:`) durante a execução dos testes, evitando a dependência de um banco PostgreSQL externo.

A atividade utiliza duas estratégias de testes:

* **Mocking:** substituição dos métodos do model `User` utilizando Vitest.
* **SQLite em memória:** execução dos testes de integração utilizando um banco temporário, criado exclusivamente durante os testes.

Os testes de integração utilizam `beforeAll`, `beforeEach` e `afterAll` para configurar, isolar e finalizar o banco de dados.

### Executar os testes

```bash
pnpm --filter backend test:run
```

### Validações

```bash
pnpm --filter backend lint
pnpm --filter backend format:check
pnpm --filter backend type-check
pnpm --filter backend test:run
pnpm --filter backend build
```

A configuração do GitHub Actions executa os testes com `NODE_ENV=test`, permitindo que o backend utilize SQLite em memória no pipeline sem depender de um banco de dados externo.
