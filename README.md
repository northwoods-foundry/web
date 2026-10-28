# meridiandata.tech

The Meridian Data LLC website: one static page plus a `security.txt`, served by GitHub Pages from `main` (root folder). No build step, no scripts, no external fonts.

| File | What it is |
|---|---|
| `index.html` | The whole site (HTML + inline CSS, light and dark mode) |
| `.well-known/security.txt` | Where to report a vulnerability ([RFC 9116](https://www.rfc-editor.org/rfc/rfc9116)) |
| `CNAME` | Tells Pages the custom domain (`meridiandata.tech`) |
| `.nojekyll` | Required: without it Pages skips dot-folders and `/.well-known/` returns 404 |

## Editing

Open `index.html` in a browser (`open index.html`), edit, reload. Push to `main` and Pages republishes in about a minute.

## One-time setup

Pages is already on (deploy from `main`, `/`, custom domain `meridiandata.tech`). What remains:

### 1. Verify the domain for the org (do this first)

Stops anyone else's GitHub repo from claiming the domain.

1. GitHub → `northwoods-foundry` org → **Settings → Pages → Add a verified domain** → `meridiandata.tech`.
2. GitHub shows a TXT record (host like `_github-pages-challenge-northwoods-foundry`, plus a value). Add it in Namecheap (step 2), then click **Verify**.

### 2. Namecheap DNS

Namecheap → Domain List → `meridiandata.tech` → **Manage → Advanced DNS → Host records**.

**Do not touch the MX or the existing TXT records** (SPF, DKIM, DMARC). They carry Google Workspace mail for `tim@meridiandata.tech`.

Delete Namecheap's default parking records if present (a `URL Redirect Record` on `@`, a `CNAME` on `www` pointing to `parkingpage.namecheap.com`). Then add:

| Type | Host | Value | TTL |
|---|---|---|---|
| A Record | `@` | `185.199.108.153` | Automatic |
| A Record | `@` | `185.199.109.153` | Automatic |
| A Record | `@` | `185.199.110.153` | Automatic |
| A Record | `@` | `185.199.111.153` | Automatic |
| CNAME Record | `www` | `northwoods-foundry.github.io.` | Automatic |
| TXT Record | *(from step 1)* | *(from step 1)* | Automatic |

Optional IPv6 (`AAAA` on `@`): `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.

### 3. HTTPS

Repo → **Settings → Pages**. Once the DNS check shows green (minutes to an hour), tick **Enforce HTTPS**. The certificate can take up to an hour after that.

### 4. Check

```bash
dig +short meridiandata.tech                 # the four 185.199.x.153 addresses
curl -sI https://meridiandata.tech | head -1 # HTTP/2 200
curl -s https://meridiandata.tech/.well-known/security.txt
dig +short MX meridiandata.tech              # unchanged Google MX records
```

Then send a test email to `tim@meridiandata.tech` to confirm mail still arrives.

## Upkeep

- **`security.txt` expires `2027-10-24`.** Update the `Expires:` line before then: an expired file is treated as invalid. Keep it under a year out.
- When a vulnerability disclosure policy page exists, add a `Policy:` line to `security.txt`.
- Sextant Gov's own domains get their own `security.txt`: cpic-backend#1093 (AWS cutover).

## To do

- **Sextant Gov proof:** once a demo tenant with fixture data is running (a made-up town and people, never a real town's data), add a screenshot to the Sextant Gov card and a "Request a demo" link. Add the product link when sextantgov.com is live.
