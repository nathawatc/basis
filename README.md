# Base TinyModern

Tiny but modern package set to define a basic Arch Linux installation.

`base-tm` provides a small, practical Arch baseline. It replaces selected legacy GNU tools with
modern shim packages, removes unused defaults, and keeps `base` compatibility via `provides`.

## Goals

- Small baseline for installs and containers
- Replace selected legacy tools with modern shims
- Remove unused default packages
- Keep behavior predictable and Arch-aligned

## Differences from `base`

| `base`               | `base-tm`                                                                       |
| -------------------- | ------------------------------------------------------------------------------- |
| `coreutils`          | `uutils-coreutils` with symlinks                                                |
| `findutils`          | `uutils-findutils` with symlinks                                                |
| `grep`               | `rg-grep`, ripgrep with [grep shim](https://codeberg.org/nathawat/rg-grep-shim) |
| `iputils`            | Removed                                                                         |
| `iproute2`           | Removed                                                                         |
| `systemd-sysvcompat` | Removed                                                                         |
| `licenses`           | Removed                                                                         |

### Rationale

- `uutils-coreutils` and `uutils-findutils` reduce GNU dependency while keeping command compatibility.
- `rg-grep` provides fast `grep`-style usage through a shim.
- `iputils` and `iproute2` are omitted for systems that do not need those networking tools.
- `systemd-sysvcompat` is omitted for systems that use `systemctl` directly.
- `licenses` is omitted to reduce footprint in an unofficial minimal package.
- `archlinux-keyring` is included explicitly for pacman trust setup.

> [!CAUTION]
>
> ### Do Not Shut Down the System
>
> Add `init=/lib/systemd/systemd` to the kernel command line before shutdown.

> [!WARNING]
>
> ### Compatibility Notes
>
> Install the original packages explicitly if your setup depends on exact behavior

#### Scope and Non-Goals

- Not a universal `base` replacement
- Not intended to preserve every legacy default
- Not intended to include complete networking tooling
