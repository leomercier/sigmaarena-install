# SigmaArena Validator Installer

Automated setup for the SigmaArena Validator stack.

This script installs and manages the validator, Redis, and Watchtower services with a single command.
It supports starting, stopping, and updating validator configuration directly from the CLI.

🧠 Overview

This setup pulls the SigmaArena Validator Docker image, starts a Redis instance, and enables automated container updates with Watchtower.

You can install everything in one line:

bash -c "$(curl -sSfL https://raw.githubusercontent.com/leomercier/sigmaarena-install/main/install)"

🧩 Components

Validator — Runs crowdform/sigmaarena-validators:latest

Redis — Local instance for task coordination

Watchtower — Automatically updates containers every 30 seconds

⚙️ Commands

The installer drops a lightweight CLI wrapper called sigma.

sigma start # starts all services (validator, redis, watchtower)
sigma stop # stops all services
sigma set --VALIDATOR_MNEMONIC "your mnemonic phrase"

Typical workflow
bash -c "$(curl -sSfL https://raw.githubusercontent.com/leomercier/sigmaarena-install/main/install)"
sigma set --VALIDATOR_MNEMONIC "xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx"
sigma start

View logs:

docker compose logs -f validator

Stop:

sigma stop

📁 Project structure
.
├── install # installer script (main entrypoint)
├── sigma # CLI helper
├── docker-compose.yml # service definitions
└── README.md

🔐 Environment variables

These are managed through .env (auto-created if missing):

Variable Description
VALIDATOR_MNEMONIC Validator seed phrase (required)
NODE_ENV Environment mode (default: production)
API_URL API endpoint
REDIS_URL Redis connection URL
HEARTBEAT_INTERVAL_MS Heartbeat interval
MAX_CONCURRENT_TASKS Number of concurrent tasks

Example .env

VALIDATOR_MNEMONIC="your mnemonic here"
NODE_ENV=production
API_URL=https://sigmaarena-conclave-788594960999.europe-west1.run.app/v1
REDIS_URL=redis://redis:6379
HEARTBEAT_INTERVAL_MS=15000
MAX_CONCURRENT_TASKS=2

🔄 Watchtower

Watchtower checks for new Docker image versions every 30 seconds and automatically redeploys containers labeled with:

com.centurylinklabs.watchtower.enable=true

🧰 Requirements

Docker with Compose plugin

curl

bash

(optional) sudo for installing the sigma binary

🧼 Cleanup

To remove all services and volumes:

sigma stop
docker volume rm sigmaarena-install_redis-data

🧾 License

MIT — use freely at your own risk.
