# CNGI Trades — simplified Unraid install

**Docker image:** `ghcr.io/jabbadahutt762/cngi-trades:latest`  
**Host → container port:** `8191 → 8080`  
**App Data:** `/mnt/user/appdata/cngi-trades → /data`  
**Planned public address:** `https://trades.clemonsnextgen.com`

## Install or refresh the template

Open **Unraid → Terminal** and paste:

```bash
mkdir -p /boot/config/plugins/dockerMan/templates-user
curl -fL --retry 3 \
  https://raw.githubusercontent.com/JabbaDaHutt762/CNGI-Unraid-Templates/main/templates/cngi-trades.xml \
  -o /boot/config/plugins/dockerMan/templates-user/my-CNGI-Trades.xml
```

Go to **Docker → Add Container → Template → CNGI-Trades**.
If the old editor is still open, exit Add Container and re-select the
updated template to remove the old mandatory fields.

**You can now click Apply with the default fields.** The only visible settings
are port 8191 and the persistent appdata folder.

The app initializes SQLite, session secret, encryption key and setup token on
first start, inside the appdata folder. No external database, family email list,
Authentik settings or brokerage credentials are needed to launch the container.

If the image can't be pulled due to a private GHCR package, run
`docker login ghcr.io -u JabbaDaHutt762` and use a GitHub personal access
token (classic) with `read:packages` at the password prompt. Never share it.

## Configure login later inside Trades

Open `http://UNRAID_IP:8191/` and select **Open initial setup**.
In the Unraid **CNGI-Trades → Console**, run:

```sh
cat /data/.setup-token
```

Copy the setup code into the Trades setup page. Create a dedicated Authentik
OAuth/OpenID application/provider with callback:

```text
https://trades.clemonsnextgen.com/auth/callback
```

Enter the public URL, Authentik discovery URL, client ID and client secret
inside Trades (the credentials are encrypted in appdata). Manage which family
members may access Trades through Authentik's application policy/group.

Later, configure Nginx Proxy Manager to serve HTTPS at
`trades.clemonsnextgen.com` and forward HTTP to the Unraid IP on port 8191.
Use the HTTPS domain for Authentik sign-in because the app uses Secure cookies.
Keep port 8191 limited to LAN and reverse proxy access.

## Verify and back up

- `http://UNRAID_IP:8191/healthz` reports `trading: not-enabled`.
- `http://UNRAID_IP:8191/readyz` reports the database is ready.
- Authentik sign-in is not enabled until you complete in-app setup.
- Each person later connects their own **Alpaca paper** account, independently.
- No automatic paper trades, real-money orders or scheduler exist yet.

Back up **all** of `/mnt/user/appdata/cngi-trades`, including the hidden
secret files, before updates. Do not delete/recreate these files; encrypted
brokerage and Authentik settings depend on them.

An advanced optional `DATABASE_URL` can point at a dedicated PostgreSQL
database when starting a fresh deployment. Migrating an existing SQLite
installation requires a separate data migration and must not be attempted
just by editing the URL.
