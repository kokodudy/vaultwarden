## Vaultwarden Service

This service runs **Vaultwarden** (a lightweight Bitwarden-compatible server) using Docker Compose.

### Service Definition

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      - SIGNUPS_ALLOWED=true
    volumes:
      - ./vw-data/:/data/
    ports:
      - 8000:80
```

### How It Works

* **Image**: Uses the official `vaultwarden/server` image.
* **Container name**: The container will be named `vaultwarden`.
* **Restart policy**: `unless-stopped` ensures the service restarts automatically after reboot or failure.

### Environment Variables

* `SIGNUPS_ALLOWED=true`
  Allows new users to register accounts. For production, consider setting this to `false` after initial setup.

### Data Persistence

* `./vw-data/:/data/`
  All Vaultwarden data (users, vaults, attachments) is stored on the host in `./vw-data/`.

⚠️ **Important**: Make sure this directory is backed up regularly.

### Network & Ports

* `8000:80`

  * Vaultwarden listens on port **80** inside the container.
  * It is exposed on **port 8000** on the host.

Access Vaultwarden via:

```
http://<HOST_IP>:8000
```

### Start the Service

```bash
docker compose up -d
```

### Stop the Service

```bash
docker compose down
```

### Basic Security Recommendations

* Disable public signups after creating your admin/user account:

  ```yaml
  SIGNUPS_ALLOWED=false
  ```
* Use a reverse proxy (Nginx, Traefik, or Cloudflare Tunnel) with HTTPS.
* Restrict access by IP if exposed to the internet.
* Keep regular backups of `vw-data/`.

### Updating Vaultwarden

```bash
docker compose pull
docker compose up -d
```

This will fetch the latest image and restart the service with existing data intact.
