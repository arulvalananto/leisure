# Hermes Agent — Docker Setup

## 1. Create directories

```bash
mkdir -p ~/work/leisure/hermes
mkdir -p ~/work/leisure/hermes-workspace
```

## 2. Create Docker Compose

Create:

```bash
~/work/leisure/hermes/docker-compose.yml
```

```bash
Contents:

services:
  hermes:
    image: nousresearch/hermes-agent
    container_name: hermes
    stdin_open: true
    tty: true
    restart: unless-stopped
    volumes:
      - ${HOME}/Library/Application Support/Hermes:/opt/data
      - ${HOME}/work/leisure/hermes-workspace:/workspace
    environment:
      TZ: Asia/Kolkata
```

## 3. Start Hermes

```bash
cd ~/work/leisure/hermes
docker compose up -d
```

## 4. Configure Hermes

Use the Docker container:

```bash
docker exec -it hermes hermes setup
```

For GitHub Copilot:

```bash
docker exec -it hermes hermes model
```

Select:

- GitHub Copilot
- Login with GitHub
- Select the desired model

## 5. Verify configuration

docker exec -it hermes hermes config

Expected:

- Provider: copilot
- Model: gpt-5-mini (or chosen model)
- Terminal backend: local

## 6. Create Hermes shell command

Add to ~/.zshrc:

```text
hermes() {
  docker exec -it hermes hermes "$@"
}
```

Reload:

```bash
source ~/.zshrc
```

Now simply run:

```bash
hermes
```

## 7. Verify workspace

Inside Hermes, ask:

What is your current directory? Does /workspace exist? List its contents. Don't create or modify anything.

Expected:

```text
/workspace exists
/workspace is empty
```

## Directory structure

```text
~/work/leisure/
├── hermes/
│   └── docker-compose.yml
└── hermes-workspace/
    └── <projects Hermes is allowed to work on>
```

## Important

- ~/.hermes contains Hermes configuration, sessions, and credentials.
- ~/work/leisure/hermes-workspace is the workspace Hermes can modify.
- Do not put real projects in the workspace until the Git workflow has been tested.
- Do not expose Hermes ports publicly unless specifically required.
