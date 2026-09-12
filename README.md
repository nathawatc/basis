# Basis

Tiny but modern package set for a basic Arch Linux.

`basis` provides a small, practical Arch baseline. `basis-devel` extends it with a
minimal package-building toolchain.

- Replace legacy tools with modern alternatives
- Remove unused default packages
- Provide `base` and `base-devel` compatibility

## Differences from `base`

| `base`               | `basis`                                     |
| -------------------- | ------------------------------------------- |
| `coreutils`          | `uutils-coreutils` + compatibility symlinks |
| `findutils`          | `uutils-findutils` + compatibility symlinks |
| `grep`               | `ugrep` + compatibility symlinks            |
| `iputils`            | Omitted                                     |
| `iproute2`           | Omitted                                     |
| `systemd-sysvcompat` | Omitted                                     |
| `licenses`           | Omitted                                     |

- `iputils` and `iproute2` are optional; install them separately when networking tools are needed.
- `systemd-sysvcompat` is unnecessary on systems managed directly through `systemctl`.
- `licenses` is excluded from this unofficial minimal package to reduce footprint.

## Differences from `base-devel`

| `base-devel` | `basis-devel`                                  |
| ------------ | ---------------------------------------------- |
| `diffutils`  | `uutils-diffutils` + compatibility symlinks    |
| `grep`       | Provided by `basis` through `ugrep`            |
| `findutils`  | Provided by `basis` through `uutils-findutils` |
| `sudo`       | Omitted; use `run0` if desired                 |
| `groff`      | Omitted                                        |
| `texinfo`    | Omitted                                        |
| `which`      | `command -v` wrapper                           |

> [!CAUTION]
>
> ### Do Not Shut Down the System
>
> Add `init=/lib/systemd/systemd` to the kernel command line before shutdown.

### Scope and Non-Goals

- Not a universal `base` replacement
- Not a universal `base-devel` replacement
- Not intended to preserve every legacy default
- Not intended for full system bootstrap
