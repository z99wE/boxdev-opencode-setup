# boxdev-opencode-setup

Run [OpenCode](https://opencode.ai) and [Pi](https://pi.dev) inside a
[boxes.dev](https://boxes.dev) cloud devbox — with full access from your phone.

This is a setup kit for pointing a boxes.dev project at OpenCode + Pi instead of
relying only on boxes.dev's built-in Codex / Claude agent threads.

## Why this exists

boxes.dev's managed agent threads currently run Claude Code and Codex only; the
`agents.runtime` option accepts `claude` and `codex` (and setups require at least
one connected account). OpenCode and Pi are not selectable as boxes.dev runtimes,
but a boxes.dev box is a full Linux machine, so the native CLIs can simply be
installed on the box and driven through a terminal or web UI.

## What's on the box

- **OpenCode v2** — installed via `curl -fsSL https://opencode.ai/v2/install | bash`
  (fallback: `npm install -g @opencode/cli`), binary at `/usr/local/bin/opencode`.
- **Pi** — installed via `curl -fsSL https://pi.dev/install.sh | sh`
  (fallback: `npm install -g @earendil-works/pi-coding-agent`), binary symlinked to
  `/usr/local/bin/pi`.

## Using OpenCode on the box

### Terminal (TUI)

```bash
dvb connect <box>   # attach to the cloud box
opencode            # OpenCode TUI
pi                   # Pi TUI
```

### Web UI

```bash
# on the box
opencode pair
# -> prints http://127.0.0.1:49374/auth/connect/<token>  (once, 5 min expiry)

# on your laptop / phone, via the boxes.dev port forward
dvb ports fwd <box> 49374     # remote 49374 -> local 49375

# then open http://127.0.0.1:49375/ in the browser
```

### Remote access (anywhere, incl. phone browser)

```bash
opencode pair --remote --yes
opencode service set password "<your-password>"
opencode service start
```

This exposes the server through OpenTunnel with an end-to-end encrypted public
URL.

## Models

OpenCode ships with free models, e.g. `opencode/big-pickle` and friends:

```bash
opencode models
```

Sign in with your own provider on the box (`opencode auth login` / `/connect`) and
pick a model with `/models`.

## Phone access

The cloud box keeps running while your laptop is closed. Wake it from anywhere:

```bash
dvb wake <box>
```

Then open the OpenCode web URL on the phone browser and sign in with the server
password.

## Costs

- Sleeping devboxes are **free**.
- Idle devboxes sleep automatically.
- Awake boxes are billed (~$0.40/box-hour on the Starter plan at 4 vCPU / 8 GiB).

## Caveats

- boxes.dev thread orchestration (desktop/mobile) is Codex/Claude only; OpenCode and
  Pi run as native tools on the box.
- Setup does not copy provider credentials onto the box; sign in on the box when
  needed.
