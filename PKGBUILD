# Maintainer: Sébastien "Seblu" Luttringer <seblu@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=4.0
pkgrel=2
pkgdesc='Btrfs filesystem utilities'
arch=('i686' 'x86_64')
depends=('glibc' 'libutil-linux' 'e2fsprogs' 'lzo' 'zlib')
makedepends=('git' 'asciidoc' 'xmlto')
url='http://btrfs.wiki.kernel.org/'
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=("git://git.kernel.org/pub/scm/linux/kernel/git/kdave/$pkgname.git#tag=v$pkgver"
        'initcpio-install-btrfs'
        'initcpio-hook-btrfs')
install=btrfs-progs.install
options=(!staticlibs)
md5sums=('SKIP'
         '7241ba3a4286d08da0d50b7176941112'
         'b09688a915a0ec8f40e2f5aacbabc9ad')

build() {
  cd $pkgname
  ./autogen.sh
  ./configure --prefix=/usr
  make
}

check() {
  cd $pkgname
 ./btrfs filesystem show
}

package() {
  cd $pkgname
  make prefix="$pkgdir"/usr install

  # install bash completion (FS#44618)
  install -Dm644 btrfs-completion "$pkgdir/usr/share/bash-completion/completions/btrfs"

  # install mkinitcpio hooks
  cd "$srcdir"
  install -Dm644 initcpio-install-btrfs "$pkgdir/usr/lib/initcpio/install/btrfs"
  install -Dm644 initcpio-hook-btrfs "$pkgdir/usr/lib/initcpio/hooks/btrfs"
}

# vim:set ts=2 sw=2 ft=sh et:
