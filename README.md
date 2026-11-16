# Blockchain Balance API

A read-oriented TypeScript API prototype that routes multi-chain asset requests through provider adapters. It separates HTTP handling, CAIP-style asset parsing, service logic, and external data providers.

## Architecture

~~~mermaid
flowchart LR
  C[Client or Postman] --> R[Express router]
  R --> H[Controller and validation]
  H --> S[Blockchain service]
  S --> P1[Alchemy provider]
  S --> P2[Bitcoin provider]
  S --> P3[CoinGecko provider]
  P1 --> E[EVM networks]
  P2 --> B[Bitcoin data]
  P3 --> M[Market data]
~~~

## Capabilities

- Read balances and transaction history by address and asset identifier.
- Request gas data for an EVM network.
- Fetch token/NFT metadata and NFT ownership information.
- Retrieve single or multiple prices and portfolio summaries.
- Use provider adapters so network-specific calls stay outside the controller.

Core routes are mounted below /api:

| Route | Purpose |
| --- | --- |
| GET /api/balance/:address/:assetId | Read an address balance |
| GET /api/history/:address/:assetId | Read transaction history |
| GET /api/gas/:networkId | Read estimated gas data |
| GET /api/price/:assetId | Read an asset price |
| GET /api/price/:assetId/history | Read price history |
| GET /api/token/metadata/:assetId | Read token metadata |
| GET /api/nft/metadata/:assetId | Read NFT metadata |
| POST /api/balances | Request multiple balances |
| POST /api/prices | Request multiple prices |
| POST /api/portfolio | Request a portfolio summary |

## Run locally

Requirements: Node.js 20 or later and npm.

    npm install
    Copy-Item .env.example .env
    npm run dev

The health endpoint is http://localhost:3000/. Configure provider keys locally in .env. Never commit that file.

For a production-style compile:

    npm run build

## Project structure

- src/router/ — endpoint registration.
- src/controller/ — request validation and HTTP responses.
- src/service/ — asset parsing and provider coordination.
- src/provider/ — Alchemy, Bitcoin, and CoinGecko adapters.
- src/data/ — network and asset contracts.

## Scope and limitations

This project exposes read APIs; it does not sign or broadcast transactions. Provider coverage and rate limits depend on the configured services. The repository is a student prototype and does not include an automated test suite or production authentication, authorization, caching, or rate limiting. Do not use it as a custody service.
