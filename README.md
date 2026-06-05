# Rustup

Mirror of [rustup](https://github.com/rust-lang/rustup), repackaged so it can be installed with [`ghr`](https://github.com/cataggar/ghr):

```sh
ghr install cataggar/rustup
rustup-init -y
```

These are the **official** `rustup-init` binaries downloaded from `https://static.rust-lang.org/rustup/` and verified against their upstream SHA-256 sidecars — not rebuilt here.
