# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=0.19.20120904
pkgrel=6
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs')
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-$pkgver.tar.xz)
install=btrfs.install

build() {
   cd $srcdir/$pkgname-$pkgver
   make CFLAGS="$CFLAGS"
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
md5sums=('d9c96e670fac7c2098a9e7ef98d4b2e2')
