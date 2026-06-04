# rustup via `ghr`

This is the `ghr` branch of [`cataggar/rustup`](https://github.com/cataggar/rustup).
It exists for one reason: to let you install **rustup** with
[`ghr`](https://github.com/cataggar/ghr) instead of piping `curl` into a shell.

The official way to install rustup is:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

That downloads the official `rustup-init` for your platform and runs it. This
branch mirrors those exact same official `rustup-init` binaries to
[GitHub Releases](https://github.com/cataggar/rustup/releases) so that `ghr`
can pick the right one for your OS/architecture and put it on your `PATH`:

```sh
ghr install cataggar/rustup
rustup-init -y
```

`ghr install` only places `rustup-init` on your `PATH`; running `rustup-init`
performs the actual installation (downloads the default toolchain, installs the
`rustup` manager into `~/.cargo/bin`, and sets up `cargo`, `rustc`, … proxies).
This mirrors `curl … | sh`, which downloads `rustup-init` and runs it for you.

> `rustup` and `rustup-init` are the **same binary**. Invoked as `rustup-init`
> it runs the installer; invoked as `rustup` it is the toolchain manager. The
> binary therefore has to keep the name `rustup-init` to bootstrap a new
> machine — which is exactly what this release ships.

## Pinning a version

```sh
ghr install cataggar/rustup@1.29.0
```

Tags match the upstream [rustup releases](https://github.com/rust-lang/rustup/releases)
(e.g. `1.29.0`).

## Platforms

The same platform set as [`cataggar/wamr`](https://github.com/cataggar/wamr) is
published, named with canonical Rust target triples:

| OS / arch          | Asset                                                |
|--------------------|------------------------------------------------------|
| Linux x86-64 (gnu) | `rustup-<ver>-x86_64-unknown-linux-gnu.tar.gz`       |
| Linux arm64 (gnu)  | `rustup-<ver>-aarch64-unknown-linux-gnu.tar.gz`      |
| Linux x86-64 musl  | `rustup-<ver>-x86_64-unknown-linux-musl.tar.gz`      |
| Linux arm64 musl   | `rustup-<ver>-aarch64-unknown-linux-musl.tar.gz`     |
| Linux riscv64      | `rustup-<ver>-riscv64gc-unknown-linux-gnu.tar.gz`    |
| macOS arm64        | `rustup-<ver>-aarch64-apple-darwin.tar.gz`           |
| macOS x86-64       | `rustup-<ver>-x86_64-apple-darwin.tar.gz`            |
| Windows x86-64     | `rustup-<ver>-x86_64-pc-windows-msvc.zip`            |
| Windows arm64      | `rustup-<ver>-aarch64-pc-windows-msvc.zip`           |

Each archive contains a single `rustup-init` (or `rustup-init.exe`) and ships
with a `.sha256` sidecar that `ghr` verifies on download.

### Picking an asset explicitly

`ghr` auto-detects your OS, architecture, **and** C library (glibc vs musl), so
a plain `ghr install cataggar/rustup` resolves to the right asset on every
supported host — including Alpine/musl and `riscv64` — as of
[ghr v0.5.0-dev.5](https://github.com/cataggar/ghr/releases/tag/v0.5.0-dev.5)
([cataggar/ghr#116](https://github.com/cataggar/ghr/issues/116)).

On an older `ghr`, or to force a specific build, name the asset directly:

```sh
ghr install cataggar/rustup/rustup-1.29.0-x86_64-unknown-linux-musl.tar.gz
ghr install cataggar/rustup/rustup-1.29.0-riscv64gc-unknown-linux-gnu.tar.gz
```

## How releases are made

These binaries are **not** rebuilt here. The
[`Release`](.github/workflows/release.yml) workflow takes an upstream rustup
version as input (e.g. `1.29.0`), downloads the official `rustup-init` binaries
from `https://static.rust-lang.org/rustup/archive/<version>/`, verifies them
against their upstream SHA-256 sidecars, repackages each as an archive named
with its Rust target triple, and publishes a GitHub Release on this repository.

To cut a new mirror release, run the **Release** workflow from the
[Actions tab](https://github.com/cataggar/rustup/actions) and enter the upstream
rustup version.
