# SigmaArena Validator Installer

Automated setup and management for the SigmaArena Validator stack.

This installer sets up the validator, Redis, and Watchtower services, and provides a CLI tool (`sigma`) for easy management: start, stop, update, configure, and monitor your validator from the command line.

## Quick Install

Run this command in your terminal:

```sh
bash -c "$(curl -sSfL https://raw.githubusercontent.com/leomercier/sigmaarena-install/main/install)"
```

## Components

- **Validator** — Runs `crowdform/sigmaarena-validators:latest`
- **Redis** — Local instance for task coordination
- **Watchtower** — Automatically updates containers every 30 seconds

## Usage

The installer places a lightweight CLI tool called `sigma` in `/usr/local/bin`.

All commands are run as:

```sh
sigma <command> [options]
```

### Main Commands

- `sigma start` — Start all services (validator, redis, watchtower)
- `sigma stop` — Stop all services
- `sigma set --KEY "value" [--ANOTHER "value2"]` — Set environment variables in `.env` (e.g. mnemonic, API URL)
- `sigma status` — Show status and health of all services
- `sigma health` — Show detailed health and recent logs
- `sigma monitor [validator|redis|watchtower]` — Tail logs for a service
- `sigma update [--compose] [--install] [--pull] [--restart]` — Update CLI, docker-compose.yml, install script, pull images, and/or restart services

### Example Workflow

```sh
# 1. Install (if not already done)
bash -c "$(curl -sSfL https://raw.githubusercontent.com/leomercier/sigmaarena-install/main/install)"

# 2. Set your validator mnemonic (required)
sigma set --VALIDATOR_MNEMONIC "xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxx"

# 3. Start the stack
sigma start

# 4. Check status
sigma status

# 5. View logs
sigma monitor validator
```

### Updating

To update the CLI and/or stack components:

```sh
# Update just the CLI
sigma update

# Update docker-compose.yml
sigma update --compose

# Update install script
sigma update --install

# Pull latest images
sigma update --pull

# Restart services after update
sigma update --restart
```

## Project Structure

After install, your project directory (default: `$HOME/sigmaarena`) will contain:

```
.
├── install               # installer script (main entrypoint)
├── sigma                 # CLI helper (copied to /usr/local/bin)
├── docker-compose.yml    # service definitions
├── .env                  # environment variables
└── README.md
```

## Environment Variables

The `.env` file (created in your project directory) supports:

- `VALIDATOR_MNEMONIC` (required)
- `NODE_ENV` (default: production)
- `API_URL` (default: https://sigmaarena-conclave-788594960999.europe-west1.run.app/v1)
- `REDIS_URL` (default: redis://redis:6379)
- `HEARTBEAT_INTERVAL_MS` (default: 15000)
- `MAX_CONCURRENT_TASKS` (default: 2)

You can set these with `sigma set --KEY "value"`.

Example `.env`:

```env
VALIDATOR_MNEMONIC="your mnemonic here"
NODE_ENV=production
API_URL=https://sigmaarena-conclave-788594960999.europe-west1.run.app/v1
REDIS_URL=redis://redis:6379
HEARTBEAT_INTERVAL_MS=15000
MAX_CONCURRENT_TASKS=2
```

## Watchtower

Watchtower checks for new Docker image versions every 30 seconds and automatically redeploys containers labeled with:

```
com.centurylinklabs.watchtower.enable=true
```

## Requirements

- Docker with Compose plugin
- curl
- bash
- (optional) sudo for installing the sigma binary

## Cleanup

To remove all services and volumes:

```sh
sigma stop
docker volume rm sigmaarena-install_redis-data
```

## License

MIT — use freely at your own risk.
