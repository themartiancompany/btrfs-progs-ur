# Maintainer: Sébastien "Seblu" Luttringer <seblu@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=4.16
pkgrel=2
pkgdesc='Btrfs filesystem utilities'
arch=('x86_64')
depends=('glibc' 'libutil-linux' 'e2fsprogs' 'lzo' 'zlib' 'zstd' 'python')
makedepends=('git' 'asciidoc' 'xmlto' 'systemd' 'python-setuptools')
url='https://btrfs.wiki.kernel.org'
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
validpgpkeys=('F2B41200C54EFB30380C1756C565D5F9D76D583B')
source=("https://www.kernel.org/pub/linux/kernel/people/kdave/btrfs-progs/btrfs-progs-v$pkgver.tar."{sign,xz}
        'initcpio-install-btrfs'
        'initcpio-hook-btrfs'
        'btrfs-scrub@.service'
        'btrfs-scrub@.timer'
        'FS#58237.patch')
install=btrfs-progs.install
options=(!staticlibs)
md5sums=('SKIP'
         '830e173b9fcef4135b0e5a2e0399344e'
         '7241ba3a4286d08da0d50b7176941112'
         'b09688a915a0ec8f40e2f5aacbabc9ad'
         '917b31f4eb90448d6791e17ea0f386c7'
         '502221c1b47a3bb2c06703d4fb90a0c2'
         '340766c706bd3de66c1cbe70fe7e2934')

prepare() {
  cd $pkgname-v$pkgver
  # apply patch from the source array (should be a pacman feature)
  local filename
  for filename in "${source[@]}"; do
    if [[ "$filename" =~ \.patch$ ]]; then
      msg2 "Applying patch ${filename##*/}"
      patch -p1 -N -i "$srcdir/${filename##*/}"
    fi
  done
  :
}

build() {
  cd $pkgname-v$pkgver
  ./configure --prefix=/usr
  make
}

check() {
  cd $pkgname-v$pkgver
 ./btrfs filesystem show
}

package() {
  cd $pkgname-v$pkgver
  make DESTDIR="$pkgdir" install

  # install bash completion (FS#44618)
  install -Dm644 btrfs-completion "$pkgdir/usr/share/bash-completion/completions/btrfs"

  # install mkinitcpio hooks
  cd "$srcdir"
  install -Dm644 initcpio-install-btrfs "$pkgdir/usr/lib/initcpio/install/btrfs"
  install -Dm644 initcpio-hook-btrfs "$pkgdir/usr/lib/initcpio/hooks/btrfs"

  # install scrub service/timer
  install -Dm644 btrfs-scrub@.service "$pkgdir/usr/lib/systemd/system/btrfs-scrub@.service"
  install -Dm644 btrfs-scrub@.timer "$pkgdir/usr/lib/systemd/system/btrfs-scrub@.timer"
}

# vim:set ts=2 sw=2 ft=sh et:
