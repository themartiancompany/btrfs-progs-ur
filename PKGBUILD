# Maintainer: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=0.20rc1.1
pkgrel=1
pkgdesc="btrfs filesystem utilities"
arch=(i686 x86_64)
depends=('glibc' 'e2fsprogs')
url="http://btrfs.wiki.kernel.org/"
replaces=('btrfs-progs-unstable')
conflicts=('btrfs-progs-unstable')
provides=('btrfs-progs-unstable')
license=('GPL2')
source=(ftp://ftp.archlinux.org/other/$pkgname/$pkgname-0.20-rc1.1.tar.xz
	initcpio-install-btrfs
	initcpio-hook-btrfs)
install=btrfs-progs.install

build() {
   cd $srcdir/$pkgname-0.20-rc1.1
   make CFLAGS="$CFLAGS"

}

package() {
   cd $srcdir/$pkgname-0.20-rc1.1
   make prefix=$pkgdir/usr install
   # fix manpage
   mkdir -p $pkgdir/usr/share/
   mv $pkgdir/usr/man $pkgdir/usr/share/man
   mkdir -p ${pkgdir}/sbin
   ln -sf /usr/bin/btrfs ${pkgdir}/sbin/btrfs
   # install mkinitcpio hooks
   install -Dm644 "$srcdir/initcpio-install-btrfs" \
     "$pkgdir/usr/lib/initcpio/install/btrfs"
   install -Dm644 "$srcdir/initcpio-hook-btrfs" \
     "$pkgdir/usr/lib/initcpio/hooks/btrfs"
}
md5sums=('4046bebaae014db1fd50cec2da0e94c0'
         '7241ba3a4286d08da0d50b7176941112'
         'b09688a915a0ec8f40e2f5aacbabc9ad')
