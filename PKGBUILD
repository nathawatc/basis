# Maintainer: nathawat <nathawat[at]noreply[dot]codeberg[dot]org>

pkgname=base-tm
pkgver=1
pkgrel=1
pkgdesc='Tiny but modern package set to define a basic Arch Linux installation'
url='https://codeberg.org/nathawat/base-tm'
arch=('any')
license=('Apache-2.0')
install=base-tm.install
provides=('base' 'coreutils' 'findutils')
conflicts=('base' 'coreutils' 'findutils')
depends=(
	# REQUIRE
	'filesystem'
	'gcc-libs'
	'glibc'
	'bash'

	# POSIX
	'uutils-coreutils'
	'uutils-findutils'
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
	local dep pkg file cmd target owner
	local links=0 skipped=0

	install -dm755 "${pkgdir}/usr/bin"

	for dep in "${depends[@]}" "${makedepends[@]}"; do
		pkg=${dep%%[<>=]*}

		[[ ${pkg} == uutils-* ]] || continue
		pacman -Qq "${pkg}" >/dev/null 2>&1 || continue

		while IFS= read -r file; do
			[[ ${file} == /usr/bin/uu-* || ${file} == /usr/bin/uutils-* ]] || continue
			[[ -e ${file} ]] || continue

			cmd=${file#/usr/bin/}
			cmd=${cmd#uu-}
			cmd=${cmd#uutils-}

			# Skip wrapper binaries, not command replacements
			[[ ${cmd} == coreutils || ${cmd} == findutils ]] && continue

			target="/usr/bin/${cmd}"
			owner=$(pacman -Qqo "${target}" 2>/dev/null || true)

			# Only replace paths owned by packages this package conflicts with.
			# Skip paths owned by util-linux, procps-ng, etc.
			if [[ -n ${owner} && ${owner} != "${pkgname}" ]]; then
				case " ${conflicts[*]} " in
					*" ${owner} "*)
						;;
					*)
						printf 'skipping %s: owned by %s\n' "${target}" "${owner}"
						((++skipped))
						continue
						;;
				esac
			fi

			if [[ -e ${pkgdir}/usr/bin/${cmd} ]]; then
				printf 'error: duplicate command generated: %s\n' "${cmd}" >&2
				return 1
			fi

			ln -s "${file}" "${pkgdir}/usr/bin/${cmd}"
			((++links))
		done < <(pacman -Qql "${pkg}" | LC_ALL=C sort)
	done

	if (( links == 0 )); then
		printf 'error: no uutils command symlinks generated\n' >&2
		return 1
	fi

	printf 'generated %d uutils symlinks, skipped %d existing commands\n' "${links}" "${skipped}"
}
