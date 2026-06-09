# mykeep.ai — landing site

The umbrella landing page for **mykeep**: a portable suite of local capabilities that any AI agent
can plug into — advanced tools that live on a USB stick, no cloud, no install. Static, dependency-free
(`index.html` only — system fonts, no external requests). Three components have shipped:
[Capsule](https://github.com/lexxx233/mykeep-capsule) (encrypted, portable memory),
[Vault](https://github.com/lexxx233/mykeep-vault) (a by-reference secrets broker), and
[Showstone](https://github.com/lexxx233/mykeep-showstone) (a portable browser the agent drives). A
**Foundry** (a local server for plug-and-play skills/tools plus the infrastructure they run on —
database, queue, cache, storage) is next.

## Host on Cloudflare Pages

**Option A — direct upload (fastest, no repo):**
```sh
cd mykeep-site
npx wrangler pages deploy . --project-name mykeep
```
First run prompts a Cloudflare login. Gives you a `*.pages.dev` URL immediately.

**Option B — Git-connected (auto-deploy on push):**
1. Push this folder to a GitHub repo (e.g. `mykeep-site`).
2. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git** → pick the repo.
3. Build command: *(none)* · Output directory: `/` (it's pure static).

## Point mykeep.ai at it (registrar = Squarespace, DNS → Cloudflare)

1. **Add the domain to Cloudflare:** dashboard → **Add a site** → `mykeep.ai` (Free plan is fine).
   Cloudflare imports the current records and gives you **two Cloudflare nameservers**
   (e.g. `xxx.ns.cloudflare.com`).
2. **Change nameservers at Squarespace** (the registrar): Squarespace **Domains → mykeep.ai →
   Nameservers → Use custom nameservers** → replace the four `ns-cloud-*.googledomains.com`
   entries with the two Cloudflare ones. (This moves DNS from Google to Cloudflare.)
3. Wait for activation (usually minutes, up to ~24–48h). Cloudflare emails you when the zone is active.
4. **Attach the domain to the Pages project:** Pages project → **Custom domains → Set up a domain** →
   `mykeep.ai` (and `www.mykeep.ai`). Cloudflare creates the records + TLS cert automatically.

Result: `https://mykeep.ai` served by Cloudflare's edge, free TLS, no origin server to run.

> Note: today mykeep.ai's A record is `198.49.23.145` (a Squarespace parking/forward IP) and DNS is on
> Google nameservers — both get replaced by the steps above.
