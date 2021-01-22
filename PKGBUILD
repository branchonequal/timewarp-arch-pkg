# Maintainer: Thomas Müller <timewarp-dev@branchonequal.com>

pkgname=timewarp
pkgver=0.0.1b1
pkgrel=1
pkgdesc="Fully automated Snapper snapshot and boot environment creation"
arch=('x86_64')
url="https://github.com/branchonequal/timewarp"
license=('MIT')
depends=('python-argh' 'python-gobject' 'python-jsonschema' 'python-pydbus' 'python-sh')
makedepends=('python-setuptools')
source=("https://github.com/branchonequal/$pkgname/archive/v$pkgver/$pkgname-v$pkgver.tar.gz")
sha512sums=('e1ef1f97cc8076420b04948c4dfc1470cd070f43b81a21e80e646b1899c8767a2fa1f777ff8f6afdb287799164ba607815dce9401100dde2c7454acb0994bf41')

build() {
  cd $srcdir/$pkgname-$pkgver
  python setup.py build
}

package() {
  cd $srcdir/$pkgname-$pkgver
  python setup.py install --root="$pkgdir" --optimize=1 --skip-build
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 data/com.branchonequal.TimeWarp.conf "$pkgdir/usr/share/dbus-1/system.d/com.branchonequal.TimeWarp.conf"
  install -Dm644 data/timewarp.conf.example "$pkgdir/etc/xdg/timewarp/timewarp.conf.example"
  install -Dm644 data/timewarp.service "$pkgdir/usr/lib/systemd/system/timewarp.service"
  install -Dm644 data/package/database/alpm/00-timewarp-create-pre-snapshot.hook "$pkgdir/etc/pacman.d/hooks/00-timewarp-create-pre-snapshot.hook"
  install -Dm644 data/package/database/alpm/zz-timewarp-create-post-snapshot.hook "$pkgdir/etc/pacman.d/hooks/zz-timewarp-create-post-snapshot.hook"

  # Install GRUB menu extension if GRUB is installed.
  if pacman -Qs grub > /dev/null; then
    install -Dm755 data/boot/loader/grub/42_timewarp "$pkgdir/etc/grub.d/42_timewarp"
  fi
}
