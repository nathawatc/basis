# Maintainer: nathawat <nathawat[at]noreply[dot]codeberg[dot]org>

pkgname=base-tm
pkgver=1
pkgrel=1
pkgdesc='Tiny but modern package set to define a basic Arch Linux installation'
url='https://codeberg.org/nathawat/base-tm'
arch=('x86_64')
license=('Apache-2.0')
install=base-tm.install
provides=('base' 'coreutils' 'findutils')
conflicts=('base' 'coreutils' 'findutils')
depends=(
    # REQUIRE
    'filesystem' 'gcc-libs' 'glibc' 'bash'
    # POSIX
    'uutils-coreutils' 'file' 'uutils-findutils' 'gawk' 'rg-grep' 'procps-ng' 'sed' 'tar'
    # TOOLSET
    'gettext' 'pciutils' 'psmisc' 'shadow' 'util-linux' 'bzip2' 'gzip' 'xz'
    # DISTRO
    'pacman' 'archlinux-keyring' 'systemd'
)
optdepends=(
  'linux: bare metal support'
)

package() {
    mkdir -p "${pkgdir}/usr/bin"

    # symlink to replace coreutils
    _cmds_coreutils=(
        arch base32 base64 basename basenc cat chcon chgrp chmod chown chroot
        cksum b2sum md5sum sha1sum sha224sum sha256sum sha384sum sha512sum
        comm cp csplit cut date dd df dir dircolors dirname du echo env expand
        expr factor false fmt fold groups head hostid id install join
        link ln logname ls mkdir mkfifo mknod mktemp mv nice nl nohup
        nproc numfmt od paste pathchk pinky pr printenv printf ptx pwd readlink
        realpath rm rmdir runcon seq shred shuf sleep sort split stat stdbuf stty
        sum sync tac tail tee timeout touch tr true truncate tsort tty uname
        unexpand uniq unlink users vdir wc who whoami yes
    )

    for cmd in "${_cmds_coreutils[@]}"; do
        ln -s "uu-${cmd}" "${pkgdir}/usr/bin/${cmd}"
    done

    # symlink to replace findutils
    _cmds_findutils=(
        find xargs
    )

    for cmd in "${_cmds_findutils[@]}"; do
        ln -s "uu-${cmd}" "${pkgdir}/usr/bin/${cmd}"
    done
}
