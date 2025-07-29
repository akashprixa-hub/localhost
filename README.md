
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

---

### Running individual containers

**Start MongoDB:**
```bash
docker run --name db -d mongo:6.0
```

Then start Rocket.Chat linked to this mongo instance:
```sh
    docker run --name rocketchat --link db:db -d rocket.chat
```
This will start a Rocket.Chat instance listening on the default Meteor port of 3000 on the container.

If you'd like to be able to access the instance directly at standard port on the host machine:

```sh
    docker run --name rocketchat -p 80:3000 --env ROOT_URL=http://localhost --link db:db -d rocket.chat
```

Then, access it via `http://localhost` in a browser.  Replace `localhost` in `ROOT_URL` with your own domain name if you are hosting at your own domain.

If you're using a third party Mongo provider, or working with Kubernetes, you need to override the `MONGO_URL` environment variable:
```sh
    docker run --name rocketchat -p 80:3000 --env ROOT_URL=http://localhost --env MONGO_URL=mongodb://mymongourl/mydb -d rocket.chat
```
