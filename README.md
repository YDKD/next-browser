# @vercel/next-browser

Headed Chromium with React DevTools pre-loaded, driven over a stateless CLI.
Built for debugging Next.js PPR shells and React component trees.

Stateless CLI → stateful daemon over a Unix socket. The daemon holds the
browser open between commands, so each CLI invocation is instant.

## Install

```bash
pnpm add -g @vercel/next-browser
```

Requires Node `>=22.18` (native TypeScript stripping, no build step).

## Usage

```bash
next-browser open http://localhost:3000
next-browser tree
next-browser ppr lock
next-browser push /dashboard
next-browser ppr unlock
next-browser close
```

## Commands

```
open <url> [--cookies-json <file>]  launch browser and navigate
close              close browser and daemon

goto <url>         full-page navigation (new document load)
push [path]        client-side navigation (interactive picker if no path)
back               go back in history
reload             reload current page
restart-server     restart the Next.js dev server (clears fs cache)

ppr lock           enter PPR instant-navigation mode
ppr unlock         exit PPR mode and show shell analysis

tree               show React component tree
tree <id>          inspect component (props, hooks, state, source)

screenshot         save full-page screenshot to tmp file
eval <script>      evaluate JS in page context

errors             show build/runtime errors
logs               show recent dev server log output
network [idx]      list network requests, or inspect one
```

## License

MIT
