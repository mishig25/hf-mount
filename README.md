# hf-mount (hf CLI extension)

An [`hf` CLI extension](https://huggingface.co/docs/huggingface_hub/en/guides/cli-extensions)
that lets you run **`hf mount`** as a thin wrapper around the standalone
[`hf-mount`](https://github.com/huggingface/hf-mount) tool — mount Hugging Face
repos and Buckets as a local filesystem, without remembering a separate binary.

```bash
hf mount start repo openai/gpt-oss-20b /tmp/model
hf mount status
hf mount stop /tmp/model
```

## Install

```bash
hf extensions install <your-username>/hf-mount
```

This installs a shell-script extension. It does **not** bundle the `hf-mount`
binary — that is installed separately (see below). After installing, `hf mount`
forwards every argument straight to the real `hf-mount` binary.

### Install the underlying `hf-mount` binary

```bash
# Homebrew (macOS / Linux)
brew install hf-mount

# or grab a prebuilt binary
# https://github.com/huggingface/hf-mount/releases

# or build from source (Rust 1.89+)
cargo install --git https://github.com/huggingface/hf-mount
```

If the binary isn't found, `hf mount` prints these instructions and exits.

## How it works

`hf` dispatches the unknown subcommand `hf mount …` to the executable
`hf-mount` shipped at the root of this repo, forwarding all extra arguments.
That script locates the **real** `hf-mount` binary on your `PATH` (plus common
install dirs like `/opt/homebrew/bin`, `~/.cargo/bin`, `~/.local/bin`), takes
care to skip itself (both are named `hf-mount`), and `exec`s it with your
arguments unchanged. So `hf mount <args>` ≡ `hf-mount <args>`.

## Uninstall

```bash
hf extensions remove mount
```

## License

MIT
