# Router Orchestrator Mainnet Service Setup

## Overview

This document provides instructions for setting up the Router Relayer Mainnet Service using Docker Swarm.

## Prerequisites

- Docker installed on your machine.
- Access to the private keys required for the service.

## Initialization

### Docker Swarm Initialization

Initialize Docker Swarm:

```bash
docker swarm init
```

### Setting Up Secrets

Store your Bitcoin private keys as Docker secrets:

```bash
echo <BITCOIN_PRIVATE_KEY> | docker secret create BITCOIN_PRIVATE_KEY -
```

Replace `<BITCOIN_PRIVATE_KEY>` with your actual private keys.

## Service Creation

Create the `router_orchestrator_mainnet_service`:

```bash
docker service create
--name router_relayer_mainnet_service
--restart-condition on-failure
--restart-delay 10s
--secret source=BITCOIN_PRIVATE_KEY,target=BITCOIN_PRIVATE_KEY
--mount type=bind,source=/Users/ganesh/repos/dfyn/router-chain/router-orchestrator/config.json,target=/router/config.json,readonly
--entrypoint "router-relayer /src/config.json"
-p 8002:8002
--env-file .env
router_relayer_image
```

## Service Management

### Listing Services

To list all running services:

```bash
docker service ls
```

### Service Status

Check the status of the `router_relayer_mainnet_service`:

```bash
docker service ps router_relayer_mainnet_service
```

### Viewing Logs

To view the logs of the service:

```bash
docker service logs router_relayer_mainnet_service
```

### Removing the Service

To remove the service:

```bash
docker service rm router_relayer_mainnet_service
```

## Additional Notes

- Ensure that the file paths and environment variables are correctly set according to your system configuration.
- Handle private keys securely and avoid exposing them in unsecured locations.
