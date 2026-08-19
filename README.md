# caity.wtf

Personal site for Caitlyn Meeks. One static page, no build step, no dependencies.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole site: markup and CSS in one file |
| `CNAME` | Tells GitHub Pages to serve at `caity.wtf`; **don't delete** |
| `.nojekyll` | Skips Jekyll processing so files starting with `_` are served as-is |

## Editing

Open `index.html`, change the text, save. To preview locally:

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Or just double-click `index.html`; it works straight from the filesystem.

Push to `main` and GitHub Pages redeploys in about a minute.

## DNS (Namecheap → Advanced DNS)

Nameservers stay on **Namecheap BasicDNS**. First delete the two parking records
(`A @ → 192.64.119.123` and `www → parkingpage.namecheap.com`); if they stay, they
win. Then add:

| Type | Host | Value | TTL |
| --- | --- | --- | --- |
| A | `@` | `185.199.108.153` | Automatic |
| A | `@` | `185.199.109.153` | Automatic |
| A | `@` | `185.199.110.153` | Automatic |
| A | `@` | `185.199.111.153` | Automatic |
| CNAME | `www` | `caitlynmeeks.github.io.` | Automatic |

Optional IPv6: add all four `AAAA` records on `@` or none, never a partial set:

```
2606:50c0:8000::153   2606:50c0:8001::153
2606:50c0:8002::153   2606:50c0:8003::153
```

### Then

1. Wait for propagation: minutes usually, up to a few hours.
   Check with `dig +short caity.wtf A`; you want the four `185.199.*` addresses.
2. GitHub issues a Let's Encrypt certificate automatically once DNS resolves.
3. Only after the cert exists, turn on **Enforce HTTPS** (repo → Settings → Pages).

### Why not Cloudflare

GitHub can only issue the certificate if it can complete an ACME challenge against
the domain. Cloudflare's proxy (orange cloud, on by default) blocks that, so the cert
never arrives. If you ever do move DNS to Cloudflare, set every record to **DNS-only**
(grey cloud), and if you later enable the proxy, use SSL mode **Full (strict)**.
Flexible plus Pages' HTTPS redirect causes an infinite redirect loop.

Cloudflare *is* worth it later for one thing: free **Email Routing**, which would give
you `hi@caity.wtf` forwarding to your inbox instead of publishing a personal address.

## Verifying

```sh
dig +short caity.wtf A          # expect four 185.199.* addresses
curl -sSI https://caity.wtf/ | head -1
gh api repos/caitlynmeeks/caity.wtf/pages \
  --jq '{cname, https_certificate: .https_certificate.state, https_enforced}'
```
