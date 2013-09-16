# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=0.20rc1.3
pkgrel=1
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs' 'lzo2')
makedepends=('git')
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=("git://git.kernel.org/pub/scm/linux/kernel/git/mason/${pkgname}.git#commit=194aa4a1bd6447bb545286d0bcb0b0be8204d79f"
	initcpio-install-btrfs
	initcpio-hook-btrfs)
install=btrfs-progs.install
options=(!staticlibs)

build() {
   cd $pkgname
   make CFLAGS="$CFLAGS"
   make CFLAGS="$CFLAGS" btrfs-select-super
}

package() {
   cd $pkgname

   make prefix=$pkgdir/usr install
   install -Dm755 btrfs-select-super $pkgdir/usr/bin

   # fix manpage
   mkdir -p $pkgdir/usr/share/
   mv $pkgdir/usr/man $pkgdir/usr/share/man

   # install mkinitcpio hooks
   install -Dm644 "$srcdir/initcpio-install-btrfs" \
     "$pkgdir/usr/lib/initcpio/install/btrfs"
   install -Dm644 "$srcdir/initcpio-hook-btrfs" \
     "$pkgdir/usr/lib/initcpio/hooks/btrfs"
}
md5sums=('SKIP'
         '7241ba3a4286d08da0d50b7176941112'
         'b09688a915a0ec8f40e2f5aacbabc9ad')
