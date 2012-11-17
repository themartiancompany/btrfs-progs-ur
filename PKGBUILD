# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=0.19.20121005
pkgrel=3
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs')
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-$pkgver.tar.xz
	initcpio-install-btrfs
	initcpio-hook-btrfs)
install=btrfs-progs.install
md5sums=('7fb014324954ee073b039dc0066de960'
         '9b73cbf5ee7f379b28489e5dc2e9027c'
         'b09688a915a0ec8f40e2f5aacbabc9ad')

build() {
   cd $srcdir/$pkgname-$pkgver
   make CFLAGS="$CFLAGS"

   # install mkinitcpio hooks
   install -Dm644 "$srcdir/initcpio-install-btrfs" \
     "$pkgdir/usr/lib/initcpio/install/btrfs"
   install -Dm644 "$srcdir/initcpio-hook-btrfs" \
     "$pkgdir/usr/lib/initcpio/hooks/btrfs"
}

package() {
   cd $srcdir/$pkgname-$pkgver
   make prefix=$pkgdir/usr install
   # fix manpage
   mkdir -p $pkgdir/usr/share/
   mv $pkgdir/usr/man $pkgdir/usr/share/man
   mkdir -p ${pkgdir}/sbin
   ln -sf /usr/bin/btrfs ${pkgdir}/sbin/btrfs
}
