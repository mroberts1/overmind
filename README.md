## Overmind

An Obsidian vault published as a static site with Quartz 5, in the Swiss
design style used by the teaching vaults. Agent-facing notes are in
[AGENTS.md](AGENTS.md).

## Getting started

```
./dev.sh      # serve at http://localhost:8080 with hot reload
./build.sh    # write the static site to public/
```

Open Obsidian on this directory. Notes go in `content/`; Quartz builds only
from there.

## Publishing

Two targets:

- GitHub Pages: push to `main`. `.github/workflows/deploy.yml` builds and
  deploys.
- Cloudflare Workers: `script/deploy-cloudflare`. Copy `.env.example` to
  `.env` and set `CF_BASE_URL` first.
