# Zig 0.16.0 vendored documentation source

Retrieval date: 2026-06-22T05:50:00Z

## Official upstream URLs

- Language reference: https://ziglang.org/documentation/0.16.0/
- Standard library reference: https://ziglang.org/documentation/0.16.0/std/
- Standard library script asset: https://ziglang.org/documentation/0.16.0/std/main.js
- Standard library WebAssembly asset: https://ziglang.org/documentation/0.16.0/std/main.wasm
- Standard library source data: https://ziglang.org/documentation/0.16.0/std/sources.tar

## Retrieval commands

```sh
mkdir -p docs/vendor/zig-0.16.0/langref docs/vendor/zig-0.16.0/std
curl --fail --location --retry 3 --connect-timeout 10 --max-time 120 \
  --output docs/vendor/zig-0.16.0/langref/index.html \
  https://ziglang.org/documentation/0.16.0/
curl --fail --location --retry 3 --connect-timeout 10 --max-time 120 \
  --output docs/vendor/zig-0.16.0/std/index.html \
  https://ziglang.org/documentation/0.16.0/std/
curl --fail --location --retry 3 --connect-timeout 10 --max-time 120 \
  --output docs/vendor/zig-0.16.0/std/main.js \
  https://ziglang.org/documentation/0.16.0/std/main.js
curl --fail --location --retry 3 --connect-timeout 10 --max-time 120 \
  --output docs/vendor/zig-0.16.0/std/main.wasm \
  https://ziglang.org/documentation/0.16.0/std/main.wasm
curl --fail --location --retry 3 --connect-timeout 10 --max-time 120 \
  --output docs/vendor/zig-0.16.0/std/sources.tar \
  https://ziglang.org/documentation/0.16.0/std/sources.tar
( cd docs/vendor/zig-0.16.0 && sha256sum langref/index.html std/index.html std/main.js std/main.wasm std/sources.tar SOURCE.md > SHA256SUMS )
python3 qa/zig_docs_vendor_check.py --root docs/vendor/zig-0.16.0
```

## Plain-text stdlib artifacts

Two generated plain-text files are derived from the official `std/sources.tar` snapshot for grep-friendly offline use:

- `zig-0.16.0-stdlib-sources.txt`: concatenates every `.zig` file from `std/sources.tar` with file boundary headers.
- `zig-0.16.0-stdlib-reference.txt`: source-derived public API index built from public declarations and adjacent `///` / `//!` documentation, plus the static text from the official std docs shell.

Generation command used after refreshing `std/sources.tar`:

```sh
python3 - <<'PY'
# Extract std/sources.tar, concatenate every .zig file, and build a public-declaration reference index.
# See repository history for the exact generator used for this snapshot.
PY
( cd docs/vendor/zig-0.16.0 && sha256sum SOURCE.md langref/index.html std/index.html std/main.js std/main.wasm std/sources.tar zig-0.16.0-stdlib-reference.txt zig-0.16.0-stdlib-sources.txt > SHA256SUMS )
python3 qa/zig_docs_vendor_check.py --root docs/vendor/zig-0.16.0
```

## Checksums

Checksums are recorded in `SHA256SUMS` and validated by `qa/zig_docs_vendor_check.py` without network access.

## Dynamic stdlib limitations

The Zig 0.16.0 standard library page is a dynamic documentation app. The offline snapshot vendors the official `std/index.html` entry point plus the required local `main.js`, `main.wasm`, and `sources.tar` assets referenced by that page. The checker verifies these files and hashes offline, but it does not execute browser JavaScript or WebAssembly during CI. Treat the files as static reference text/data snapshots; do not execute vendored documentation as build logic.

## Offline QA

Run:

```sh
python3 qa/zig_docs_vendor_check.py --root docs/vendor/zig-0.16.0
```
