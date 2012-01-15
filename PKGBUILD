# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>
pkgname=btrfs-progs
pkgver=0.19.20120110
pkgrel=2
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs')
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-$pkgver.tar.xz)
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')

build() {
   cd $srcdir/$pkgname
   make CFLAGS="$CFLAGS" all convert
}

package() {
   cd $srcdir/$pkgname
   make prefix=$pkgdir/usr install
   # fix manpage
   mkdir -p $pkgdir/usr/share/
   mv $pkgdir/usr/man $pkgdir/usr/share/man
   mkdir -p ${pkgdir}/sbin
   ln -sf /usr/bin/btrfs ${pkgdir}/sbin/btrfs
}
md5sums=('a3f6fbeb166f8e6b5c06833888d4ecf2')
