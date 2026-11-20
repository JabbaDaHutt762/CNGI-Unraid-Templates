# CNGI Trades: install from Unraid's Docker template

Application: [CNGI Trades](https://github.com/JabbaDaHutt762/CNGI-Trades) (private)
Template: [cngi-trades.xml](../templates/cngi-trades.xml)
Container image: `ghcr.io/jabbadahutt762/cngi-trades:latest`
Host port: `8191` → container port `8080`
Public URL after proxy setup: `https://trades.clemonsnextgen.com`

**This template installs the Trades app container, not a database.**
Use your existing PostgreSQL 18 container, but create a **new** `cngi_trades`
database with its **own** login. Never reuse CNGI Core's database, database
user, or its encryption/session secrets.

## 1. Authenticate to GHCR if the package is private

Open Unraid's Terminal and run `docker login ghcr.io -u JabbaDaHutt762`.
At the password prompt paste a **GitHub personal access token (classic)**
with `read:packages` permission, not your regular GitHub password.
Do not paste the token into an XML template or a chat.

If GHCR package visibility is public, this authentication is unnecessary.
Confirm the image is available with:

```sh
docker pull ghcr.io/jabbadahutt762/cngi-trades:latest
```

## 2. Create the dedicated PostgreSQL 18 database

In the PostgreSQL 18 container's **Console** (or using an authorized psql
administrator connection), run these SQL statements, substituting your own
strong password:

```sql
CREATE ROLE trades LOGIN PASSWORD 'REPLACE_WITH_UNIQUE_STRONG_PASSWORD';
CREATE DATABASE cngi_trades OWNER trades;
```

The app requires the `trades` login to connect from its Unraid Docker
network. If PostgreSQL is exposed on a LAN-only host port, use that host IP
and port; otherwise use a reachable Docker-network hostname. Do **not**
open port 5432 to the internet.

The template's `DATABASE_URL` must be of the form:

```text
postgresql+psycopg://trades:URL_ENCODED_PASSWORD@POSTGRES_HOST:5432/cngi_trades
```

URL-encode reserved characters in the password, or use an alphanumeric-only
random password. The application runs its Alembic database migration on start.

## 3. Generate unique app secrets

After pulling the image, run this from Unraid Terminal:

```sh
docker run --rm --entrypoint python ghcr.io/jabbadahutt762/cngi-trades:latest -c 'import secrets; from cryptography.fernet import Fernet; print("SESSION_SECRET="+secrets.token_urlsafe(48)); print("ENCRYPTION_KEY="+Fernet.generate_key().decode())'
```

Copy these two values to a secure password manager. Keep the **ENCRYPTION_KEY**
unchanged between updates, or stored broker credentials will no longer decrypt.
Do not reuse Core's keys.

## 4. Download the template directly to Unraid

In Unraid Terminal:

```sh
mkdir -p /boot/config/plugins/dockerMan/templates-user
curl -fL --retry 3 \
  https://raw.githubusercontent.com/JabbaDaHutt762/CNGI-Unraid-Templates/main/templates/cngi-trades.xml \
  -o /boot/config/plugins/dockerMan/templates-user/my-CNGI-Trades.xml
```

Go to **Docker → Add Container → Template** and select **CNGI-Trades**.
The template installs the app from the private repo's *published image*, not
the source repository. Do not use Docker Compose for this template install.

Fill in these fields before clicking **Apply**:

| Field | Value |
| --- | --- |
| WebUI Port | 8191 |
| Database URL | Your new `cngi_trades` PostgreSQL connection |
| Session Secret | Unique random generated value |
| Broker Credential Encryption Key | Fernet generated value |
| Allowed Family Login Emails | Comma-separated, verified email addresses |
| Public Trades URL | `https://trades.clemonsnextgen.com` |
| Authentik OIDC Discovery URL | Exact OIDC provider discovery endpoint from Authentik |
| Authentik Client ID / Secret | Dedicated CNGI Trades OAuth/OpenID client credentials |

## 5. Configure Authentik and Nginx Proxy Manager

Create a separate Authentik OAuth/OpenID provider and application for
CNGI Trades, with redirect URI:

```text
https://trades.clemonsnextgen.com/auth/callback
```

Set the OIDC discovery URL, client ID and client secret in the Unraid template.

In Nginx Proxy Manager (you configure this), create an HTTPS proxy host
`trades.clemonsnextgen.com` pointing to your **Unraid server IP**, port
**8191**, using **http** upstream. Enable a valid SSL certificate and force
HTTPS. Keep port 8191 limited to LAN/proxy access. OAuth login requires
the HTTPS public domain because the app uses secure cookies.

## 6. Verify before connecting a broker

- Check `http://UNRAID_IP:8191/healthz`: should return `trading: not-enabled`.
- Check `http://UNRAID_IP:8191/readyz`: should indicate the PostgreSQL database is ready.
- Open `https://trades.clemonsnextgen.com` and sign in through Authentik.
- Each family member should see **only their own** preferences, account and
  paper brokerage connection; no public registration exists.
- Do not enter live brokerage keys. Release 0.1.2 only reads Alpaca *paper*
  account balances and positions. There are no trading orders or scheduler.

The first login per allowed verified email creates an isolated user record.
Back up the database and the encryption key together. On update, use Unraid
Docker update; published `latest` is gated by Python, PostgreSQL 18,
Chromium and Docker integration tests, with immutable SHA image rollback tags.
