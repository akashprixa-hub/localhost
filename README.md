
# Rocket.Chat Compose Files

## TLDR;

- Rocket.Chat will be listening on [http://localhost:3000](http://localhost:3000)
- Grafana will be listening on [http://localhost:5050](http://localhost:5050)

1. Clone: `git clone --depth 1 https://github.com/RocketChat/compose.git`
2. cd to the cloned dir: `cd compose`
3. Copy the example environment file: `cp .env.example .env`
4. Edit .env file and update values
5. Start the stack: `docker compose -f compose-monitoring.yml -f compose-traefik.yml -f compose.yml up -d`

## Getting Started

First, clone this repository:

```bash
git clone --depth 1 https://github.com/RocketChat/compose.git
```

---


### Docker/Podman Compose


For deploying the recommended stack with Rocket.Chat, Traefik, MongoDB, NATS, and Prometheus for monitoring:

1. **Configure environment variables:**
   - Copy the example environment file:
     ```bash
     cp .env.example .env
     ```
   - Edit `.env` to fit your deployment. Recommended changes:
     ```env
     REG_TOKEN=            # Rocket.Chat Cloud registration token (optional)
     TRAEFIK_PROTOCOL=http       # Set to 'https' to enable HTTPS with Traefik (recommended for internet exposure)
     LETSENCRYPT_EMAIL=    # Email for Let's Encrypt certificate
     DOMAIN=localhost      # Domain for Rocket.Chat
     GRAFANA_DOMAIN=grafana.localhost # Domain for Grafana
     ROOT_URL=http://localhost # Should match your domain; use https if enabled
     ```

2. **Start the stack:**
   - With Docker Compose:
     ```bash
     docker compose \
       -f compose-monitoring.yml \
       -f compose-traefik.yml \
       -f compose.yml \
       up -d
     ```
   - Or with Podman Compose:
     ```bash
     podman compose \
       -f compose-monitoring.yml \
       -f compose-traefik.yml \
       -f compose.yml \
       up -d
     ```

   This will launch all containers. Rocket.Chat will be available at [http://localhost](http://localhost), and Grafana at [http://grafana.localhost](http://grafana.localhost).
   > **Note:** If deploying to a custom domain, update `ROOT_URL` and related variables accordingly.

3. **Stop the stack:**
   ```bash
   podman compose \
     -f compose-monitoring.yml \
     -f compose-traefik.yml \
     -f compose.yml \
     down
   ```

---

### Customizing the stack

To exclude components (e.g., MongoDB or Prometheus), simply remove their compose files from the command. For example, to deploy Rocket.Chat with Traefik only:

```bash
podman compose \
  -f compose-traefik.yml \
  -f compose.yml \
  up -d
```
