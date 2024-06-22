# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Sébastien "Seblu" Luttringer <seblu@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

_docs=true
_udev=true
_os="$( \
  uname \
    -o)"
[[ "${_os}" == "Android" ]] && \
  _docs=false \
  _udev=false
pkgname=btrfs-progs
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
  # 'git'
  'asciidoc'
  'xmlto'
  'systemd'
  'python'
  'python-setuptools'
  'e2fsprogs' 
  'reiserfsprogs'
  'python-sphinx'
  'python-sphinx_rtd_theme'
)
depends=(
  'glibc'
  'util-linux-libs'
  'lzo'
  'zlib'
  'zstd'
  'libgcrypt'
  'systemd-libs'
)
optdepends=(
  'python: libbtrfsutil python bindings'
  'e2fsprogs: btrfs-convert'
  'reiserfsprogs: btrfs-convert'
)
url='https://btrfs.readthedocs.io'
replaces=(
  'btrfs-progs-unstable'
)
conflicts=(
  'btrfs-progs-unstable'
)
provides=(
  'btrfs-progs-unstable'
)
license=(
  'GPL-2.0-only'
)
validpgpkeys=(
  'F2B41200C54EFB30380C1756C565D5F9D76D583B'
)
source=(
  "https://www.kernel.org/pub/linux/kernel/people/kdave/btrfs-progs/btrfs-progs-v$pkgver.tar."{sign,xz}
  'initcpio-install-btrfs'
  'initcpio-hook-btrfs'
  'btrfs-scrub@.service'
  'btrfs-scrub@.timer'
)
install=btrfs-progs.install
options=(
  !staticlibs)
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
    _configure_opts=()
  _configure_opts=(
    --prefix=/usr
    --with-crypto=libgcrypt
  )
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
  ./configure \
    "${_configure_opts[@]}"
  make
}

check() {
  cd \
    "${pkgname}-v${pkgver}"
 ./btrfs \
   filesystem \
     show
}

package() {
  cd \
    "${pkgname}-v${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    install \
    install_python
  # install bash completion (FS#44618)
  install \
    -Dm644 \
    btrfs-completion \
    "${pkgdir}/usr/share/bash-completion/completions/btrfs"
  # install mkinitcpio hooks
  cd "$srcdir"
  install \
    -Dm644 \
    initcpio-install-btrfs \
    "$pkgdir/usr/lib/initcpio/install/btrfs"
  install \
    -Dm644 \
    initcpio-hook-btrfs \
    "${pkgdir}/usr/lib/initcpio/hooks/btrfs"
  # install scrub service/timer
  install \
    -Dm644 \
    btrfs-scrub@.service \
    "${pkgdir}/usr/lib/systemd/system/btrfs-scrub@.service"
  install \
    -Dm644 \
    btrfs-scrub@.timer \
    "${pkgdir}/usr/lib/systemd/system/btrfs-scrub@.timer"
}

# vim:set ts=2 sw=2 ft=sh et:
