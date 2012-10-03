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
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-$pkgver.tar.xz
	initcpio-install-btrfs
	initcpio-hook-btrfs)
install=btrfs.install

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
md5sums=('d9c96e670fac7c2098a9e7ef98d4b2e2'
         '2d3df276f80bb09813f56a56d6f93ddd'
         '9fb35142755b477a96cb7292f3d64839')
