# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Sébastien "Seblu" Luttringer <seblu@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

_py='python'
_docs=true
_udev=true
_convert=true
_python=true
_os="$( \
  uname \
    -o)"
[[ "${_os}" == "Android" ]] && \
  _docs=false \
  _udev=false \
  _convert=false
_pkg="btrfs"
pkgname="${_pkg}-progs"
pkgver=6.9
pkgrel=1
pkgdesc='Btrfs filesystem utilities'
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'mips'
  'i686'
  'powerpc'
  'armv7l'
)
makedepends=(
  'xmlto'
)
[[ "${_python}" == 'true' ]] && \
  makedepends+=(
    "${_py}"
    "${_py}-setuptools"
  )
[[ "${_docs}" == 'true' ]] && \
  makedepends+=(
    'asciidoc'
    "${_py}-sphinx"
    "${_py}-sphinx_rtd_theme"
  )
[[ "${_udev}" == 'true' ]] && \
  makedepends+=(
    'systemd'
  )
[[ "${_convert}" == 'true' ]] && \
  makedepends+=(
    'e2fsprogs' 
    'reiserfsprogs'
  )
depends=(
  'glibc'
  'util-linux-libs'
  'lzo'
  'zlib'
  'zstd'
  'libgcrypt'
)
[[ "${_udev}" == 'true' ]] && \
  depends+=(
    'systemd-libs'
  )
optdepends=(
  "${_py}: libbtrfsutil python bindings"
  'e2fsprogs: btrfs-convert'
  'reiserfsprogs: btrfs-convert'
)
url="https://${_pkg}.readthedocs.io"
replaces=(
  "${pkgname}-unstable"
)
conflicts=(
  "${pkgname}-unstable"
)
provides=(
  "${pkgname}-unstable"
)
license=(
  'GPL-2.0-only'
)
validpgpkeys=(
  'F2B41200C54EFB30380C1756C565D5F9D76D583B'
)
source=(
  "https://www.kernel.org/pub/linux/kernel/people/kdave/${pkgname}/${pkgname}-v${pkgver}.tar."{sign,xz}
  "initcpio-install-${_pkg}"
  "initcpio-hook-${_pkg}"
  "${_pkg}-scrub@.service"
  "${_pkg}-scrub@.timer"
)
install="${pkgname}.install"
options=(
  !staticlibs
)
sha256sums=(
  'SKIP'
  '7e14a5d597f323dd7d1b453e3a4e661a7e9f07ea060efbff4f76ff8315917de8'
  'bbe60b35d1b1e2efc1308a8f54f1fdc6808240a81c5f5b4d75321b7ee86e41f4'
  '35efeee8590d6d60c711ae9cdc918e4841ab61d10cb02359e65e36ebff95ffc5'
  'eaa7af92d28bfa8940bb551560fd7be777f9f175292eaa72b5f6ef00fb240252'
  '9a0b6cc23f7bd97b83b6c38dd2b4e4373fead8bd3ccfb82a47c72971e9d6f8ad'
)

prepare() {
  cd \
    "${pkgname}-v${pkgver}"
  # apply patch from the source array
  # (should be a pacman feature)
  local \
    src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ "${src}" = *.patch ]] || \
      continue
    echo \
      "Applying patch $src..."
    patch \
      -Np1 < \
      "../$src"
  done
}

build() {
  local \
    _configure_opts=() \
    _cflags=()
  _cflags=(
    "${CFLAGS}"
  )
  _configure_opts=(
    --prefix=/usr
    --with-crypto=libgcrypt
  )
  if [[ "${_os}" == 'Android' ]]; then
    _cflags+=(
      -Wno-implicit-function-declaration
    )
  fi
  if [[ "${_docs}" == 'false' ]]; then
    _configure_opts+=(
      --disable-documentation
    )
  fi
  if [[ "${_udev}" == 'false' ]]; then
    _configure_opts+=(
      --disable-libudev
    )
  fi
  cd \
    "${pkgname}-v${pkgver}"
  CFLAGS="${_cflags[*]}" \
  ./configure \
    "${_configure_opts[@]}"
  CFLAGS="${_cflags[*]}" \
  make
}

check() {
  cd \
    "${pkgname}-v${pkgver}"
 "./${_pkg}" \
   filesystem \
     show
}

package() {
  local \
    _make_opts=()
  cd \
    "${pkgname}-v${pkgver}"
  _make_opts=(
    DESTDIR="${pkgdir}"
    install
  )
  if [[ "${_python}" == 'true' ]]; then
    _make_opts+=(
      install_python
    )
  fi
  make \
    "${_make_opts[@]}"
  # install bash completion (FS#44618)
  install \
    -Dm644 \
    btrfs-completion \
    "${pkgdir}/usr/share/bash-completion/completions/btrfs"
  # install mkinitcpio hooks
  cd \
    "${srcdir}"
  install \
    -Dm644 \
    "initcpio-install-${_pkg}" \
    "${pkgdir}/usr/lib/initcpio/install/${_pkg}"
  install \
    -Dm644 \
    "initcpio-hook-${_pkg}" \
    "${pkgdir}/usr/lib/initcpio/hooks/${_pkg}"
  # install scrub service/timer
  install \
    -Dm644 \
    "${_pkg}-scrub@.service" \
    "${pkgdir}/usr/lib/systemd/system/${_pkg}-scrub@.service"
  install \
    -Dm644 \
    "${_pkg}-scrub@.timer" \
    "${pkgdir}/usr/lib/systemd/system/${_pkg}-scrub@.timer"
}

# vim:set ts=2 sw=2 ft=sh et:
