# caity.wtf

Personal site for Caitlyn Meeks. One static page, no build step, no dependencies.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole site — markup and CSS in one file |
| `CNAME` | Tells GitHub Pages to serve at `caity.wtf` — **don't delete** |
| `.nojekyll` | Skips Jekyll processing so files starting with `_` are served as-is |

## Editing

Open `index.html`, change the text, save. To preview locally:

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Or just double-click `index.html` — it works straight from the filesystem.

Push to `main` and GitHub Pages redeploys in about a minute.

## DNS (Namecheap)

The apex domain `caity.wtf` needs four `A` records and the `www` subdomain a `CNAME`:

| Type | Host | Value |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `caitlynmeeks.github.io.` |

Set Namecheap's nameservers to **Namecheap BasicDNS** and remove any parking/redirect
records, or the `A` records above won't take effect.
