# @vercel/next-browser

Headed Chromium with React DevTools pre-loaded, driven over a stateless CLI.
Built for debugging Next.js PPR shells and React component trees.

Stateless CLI → stateful daemon over a Unix socket. The daemon holds the
browser open between commands, so each CLI invocation is instant.

## Install

```bash
pnpm add -D @vercel/next-browser
```

Requires Node `>=22.18` (native TypeScript stripping, no build step).

## Usage

```bash
npx next-browser open http://localhost:3000
npx next-browser tree
npx next-browser ppr lock
npx next-browser push /dashboard
npx next-browser ppr unlock
npx next-browser close
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

## How it works

- Daemon spawns on first CLI call, listens on `~/.next-browser/default.sock`
- Chromium launched via `launchPersistentContext` with the React DevTools
  extension sideloaded (`--load-extension`)
- `installHook.js` pre-injected via `addInitScript` to beat the content-script
  race — DevTools hook is always available on first render
- Component tree read directly from `__REACT_DEVTOOLS_GLOBAL_HOOK__` by
  intercepting the `operations` wire format
- Source locations resolved via Next's `/__nextjs_original-stack-frames`
  endpoint, falling back to raw source-map lookup for framework code
- PPR lock holds `@next/playwright`'s `instant()` callback open across CLI
  calls so you can poke at the frozen shell interactively

## License

MIT
