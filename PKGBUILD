# Maintainer: nathawat <nathawat[at]noreply[dot]codeberg[dot]org>

pkgname=base-tm
pkgver=1
pkgrel=1
pkgdesc='Tiny but modern package set to define a basic Arch Linux installation'
url='https://codeberg.org/nathawat/base-tm'
arch=('any')
license=('Apache-2.0')
install=base-tm.install
provides=('base')
conflicts=('base')
depends=(
	# REQUIRE
	'filesystem'
	'gcc-libs'
	'glibc'
	'bash'

	# POSIX
	'uutils-coreutils-shim'
	'uutils-findutils-shim'
	'file'
	'gawk'
	'rg-grep'
	'procps-ng'
	'sed'
	'tar'

	# TOOLSET
	'bzip2'
	'gzip'
	'xz'
	'gettext'
	'pciutils'
	'psmisc'
	'shadow'
	'util-linux'

	# DISTRO
	'pacman'
	'archlinux-keyring'
	'systemd'
)
optdepends=(
	'linux: bare metal support'
)

package() {
	:
}
