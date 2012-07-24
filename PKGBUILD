# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>
pkgname=btrfs-progs
pkgver=0.19.20120328
pkgrel=4
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs')
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-$pkgver.tar.xz
        70-btrfs.rules
        initcpio-install-btrfs
        initcpio-hook-btrfs)
md5sums=('f4504e73cf9254779b78d5b2318ac570'
         '345c62c8b267082361729ca5b647518f'
         'e5186ec3fe8a809b7473470128d1c4ab'
         '9fb35142755b477a96cb7292f3d64839')

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

   # add udev rule
   install -Dm644 "$srcdir/70-btrfs.rules" "$pkgdir/usr/lib/udev/rules.d/70-btrfs.rules"

   # install mkinitcpio hooks
   install -Dm644 "$srcdir/initcpio-install-btrfs" \
     "$pkgdir/usr/lib/initcpio/install/btrfs"
   install -Dm644 "$srcdir/initcpio-hook-btrfs" \
     "$pkgdir/usr/lib/initcpio/hooks/btrfs"
}
