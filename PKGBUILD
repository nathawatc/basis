# Maintainer: nathawat <nathawat[at]noreply[dot]codeberg[dot]org>

pkgbase=basis
pkgname=(
	'basis'
	'basis-devel'
)
pkgver=1
pkgrel=1
url='https://codeberg.org/nathawat/basis'
arch=('any')
license=('Unlicense')
source=('which')
b2sums=('992aa75d65a46436ecc7d87e93de819526292dba1e87cc520661a82108042e725f953e6a60baf9509c65e63228782acbf797a441e7b09263975ae36f9bfb48a9')

# Required globally because package functions inspect/run these tools while packaging.
makedepends=(
	'uutils-coreutils'
	'uutils-findutils'
	'uutils-diffutils'
)

_deps_basis=(
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
	'ugrep'
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

_deps_basis_devel=(
	'autoconf'
	'automake'
	'binutils'
	'bison'
	'debugedit'
	'uutils-diffutils'
	'fakeroot'
	'gcc'
	'flex'
	'libtool'
	'm4'
	'make'
	'patch'
	'pkgconf'
	'systemd>=256'
	'sh'
)

package_basis() {
	pkgdesc='Tiny but modern package set to define a basic Arch Linux installation'
	install='basis.install'
	depends=(
		"${_deps_basis[@]}"
	)
	optdepends=(
		'linux: bare metal support'
	)
	provides=(
		'base-tm'
		'base'
		'coreutils'
		'findutils'
		'grep'
	)
	conflicts=(
		'base-tm'
		'base'
		'coreutils'
		'findutils'
		'grep'
	)

	local dep pkg file cmd target owner
	local links=0 skipped=0

	install -dm755 "${pkgdir}/usr/bin"

	for dep in "${depends[@]}"; do
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

	for cmd in grep egrep fgrep; do
		ln -s /usr/bin/ugrep "${pkgdir}/usr/bin/${cmd}"
	done

	printf 'generated %d uutils symlinks, skipped %d existing commands\n' "${links}" "${skipped}"
}

package_basis-devel() {
	pkgdesc='Tiny but modern basic tools to build Arch Linux packages'
	unset install
	depends=(
		"basis=${pkgver}"
		"${_deps_basis_devel[@]}"
	)
	provides=(
		'base-devel'
		'diffutils'
		'which'
	)
	conflicts=(
		'base-devel'
		'diffutils'
		'which'
	)

	local cmd raw
	local -a cmds=()

	install -dm755 "${pkgdir}/usr/bin"

	raw=$(diffutils 2>&1 || :)

	mapfile -t cmds < <(
		printf '%s\n' "${raw}" |
			awk '
				/^Currently defined functions:/ {
					found = 1
					next
				}

				found && NF {
					seen = 1
					gsub(/,/, " ")
					for (i = 1; i <= NF; i++) {
						if ($i ~ /^[A-Za-z0-9._+-]+$/)
							print $i
					}
					next
				}

				found && seen && !NF {
					exit
				}
			' |
			LC_ALL=C sort -u
	)

	if ((${#cmds[@]} == 0)); then
		printf 'error: failed to detect uutils-diffutils commands\n' >&2
		printf '%s\n' "${raw}" >&2
		return 1
	fi

	for cmd in "${cmds[@]}"; do
		if [[ -e "${pkgdir}/usr/bin/${cmd}" ]]; then
			printf 'error: duplicate command generated: %s\n' "${cmd}" >&2
			return 1
		fi

		ln -s /usr/bin/diffutils "${pkgdir}/usr/bin/${cmd}"
	done

	install -Dm755 "${srcdir}/which" "${pkgdir}/usr/bin/which"

	printf 'generated %d diffutils symlinks\n' "${#cmds[@]}"
}
