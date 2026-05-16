# Base TinyModern

Tiny but modern package set to define a basic Arch Linux installation.

`base-tm` keeps the scope small: a minimal, practical baseline for systems that
do not need legacy defaults from the official `base` package. It trims packages
that are unused in modern setups and swaps a few tools for faster, more
streamlined alternatives.

## Goals

- Small, modern baseline for fresh installs or containers
- Remove legacy and unused components
- Keep behavior predictable and Arch-aligned

## Differences from `base`

- `base` uses GNU `coreutils` and `findutils`.
- `base` uses `grep`, but `base-tm` uses `ripgrep` with a [shim](https://codeberg.org/nathawat/rg-grep-shim) translation layer for the `grep` command. <!-- rumdl-disable-line MD013 -->
- `base` includes `iputils` and `iproute2`; removed in `base-tm` since it's not used by `iwd` and `systemd-networkd`.
- `systemd-sysvcompat` removed; use latest `systemctl` command.
- `licenses` removed since this is an unofficial package to reduce disk footprint.

### Rationale (Short)

- GNU core utilities remain part of `base`; `base-tm` keeps focus on a slimmer footprint.
- `ripgrep` is fast and reliable; the shim keeps `grep`-style command compatibility.
- Legacy networking tools are unnecessary if `iwd` and `systemd-networkd` are not in use.
- `systemd-sysvcompat` is not required for systems that standardize on modern `systemctl` workflows.
- `licenses` is omitted to save disk space in an unofficial package.

> [!CAUTION]
> ### Do Not Shutdown The system
>
> Ensure you have added `init=/lib/systemd/systemd` to kernel command line before shutdown.

> [!WARNING]
> ### Compatibility Notes
>
> - If you depend on `grep`-specific behavior, confirm the shim behavior meets your scripts needs.
> - If your stack expects `iputils` or `iproute2`, install them explicitly or switch to `mtr` for ping.
> - For SysV-style init scripts, install `systemd-sysvcompat` manually.

#### Scope and Non-Goals

- Not a drop-in replacement for every `base` use case
- Not intended to preserve legacy defaults
- Not intended to include complete networking tooling
