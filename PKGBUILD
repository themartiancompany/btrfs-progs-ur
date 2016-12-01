# Maintainer: Sébastien "Seblu" Luttringer <seblu@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>

pkgname=btrfs-progs
pkgver=4.8.5
pkgrel=1
pkgdesc='Btrfs filesystem utilities'
arch=('i686' 'x86_64')
depends=('glibc' 'libutil-linux' 'e2fsprogs' 'lzo' 'zlib')
makedepends=('git' 'asciidoc' 'xmlto' 'systemd')
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
        'btrfs-scrub@.timer')
install=btrfs-progs.install
options=(!staticlibs)
sha256sums=('SKIP'
            'd247b1c022aca5def3415bfa51e00c566cce36660a3ee517d4b6a1af911a08ef'
            'bbe60b35d1b1e2efc1308a8f54f1fdc6808240a81c5f5b4d75321b7ee86e41f4'
            '35efeee8590d6d60c711ae9cdc918e4841ab61d10cb02359e65e36ebff95ffc5'
            '6c341018a50d28c1c16631ae1f2ded2c74ddc5c51851f0eb675127cf27e4fdc6'
            '4c5585974a431561b6db9e2b439cb40ec1c633325b600a7c9640e7b6640134cd')

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
