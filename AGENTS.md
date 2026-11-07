# AGENTS.md

Technical notes for agents working on this Obsidian vault, published as a
static site with Quartz 5. Human-facing orientation lives in `README.md`.
Update this file when a change invalidates something here.

## Layout

```
./              Obsidian vault root
  content/      what Quartz builds from. Notes and assets go here
  .quartz/      the Quartz 5 install, vendored, hidden from Obsidian
  public/       build output, gitignored
  wrangler.jsonc  Cloudflare Workers static assets config
```

Derived from `~/Obsidian/quartz-template`, so the Quartz and Swiss-style notes
in the sibling vaults (for example `fsu-interactive-media-fa26/AGENTS.md`)
apply here: IBM Plex Sans/Mono, stone palette, Swiss red accent, styling in
`.quartz/quartz/styles/custom.scss`, config in `.quartz/quartz.config.yaml`.

## Running it

Use the scripts, never `npx quartz`: the repo's bin is not linked into
`node_modules/.bin`, so npx fetches an unrelated registry package.

| Task              | Command                  |
| ----------------- | ------------------------ |
| Serve with HMR    | `./dev.sh` (8080, 3003)  |
| Build to public   | `./build.sh`             |
| Deploy Cloudflare | `script/deploy-cloudflare` |

Never run `build.sh` while `dev.sh` is running; both write `public/`.
Everything the site serves must live under `content/`. New asset files need a
dev server restart. A frontmatter YAML error kills the dev server; quote
titles containing colons.

## Deployment

Two hosts with different base URLs:

- GitHub Pages serves under a subpath. The committed `baseUrl` is
  `mroberts1.github.io/overmind`, used by `.github/workflows/deploy.yml` on
  push to `main`. Do not commit or push while iterating; a push is a deploy.
- Cloudflare serves at a domain root. `script/deploy-cloudflare` swaps
  `CF_BASE_URL` (from `.env`, gitignored) into the config, builds, runs
  `npx wrangler deploy`, then restores the config. If it is interrupted,
  check `git diff .quartz/quartz.config.yaml`.

`wrangler.jsonc` uses the Workers static assets form, not Pages. The
`@quartz-community/cname` plugin should stay disabled on a `github.io`
subpath.

`.quartz/.node-version` is pinned to match every sibling vault; change it only
to move them all together.
