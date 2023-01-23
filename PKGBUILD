# Maintainer: Thomas Thérouanne <thomas@therouanne.com>

pkgname=icingadb
pkgver=1.1.0
pkgrel=1
pkgdesc="An open source host, service and network monitoring program - icingadb"
license=('GPL v2.0')
arch=('x86_64')
url="https://icinga.com/"
depends=('icinga2')
makedepends=('go')
backup=(etc/icingadb/config.yml)
install='icingadb.install'
source=("https://github.com/Icinga/$pkgname/archive/v$pkgver.tar.gz"
        "$pkgname.service"
        "$pkgname.sysusers")
sha256sums=('721316166173892f8ba7d7c4f77af3b8576477be108d4a7f98ae14c29732ac31'
	    '26f6ee590d433f8933add1b4056c1b2c864ddca15824f8fc47411733eeefad05'
	    '84b01b9e8c98193b870944f30f9d16cd0e5d63a8e8cfe5537fb1af4a6100b30e')

build() {
  cd "$pkgname-$pkgver"
  go build -buildvcs=false -trimpath ./cmd/icingadb
}

package() {
  mkdir -p $pkgdir/{etc/icingadb,var/lib/icingadb}
  install -Dm755 "$pkgname-$pkgver/$pkgname" "$pkgdir/usr/bin/$pkgname"
  cp -a "$pkgname-$pkgver/schema" "$pkgdir/var/lib/icingadb/schema"
  
  # config files for creating users and groups
  install -Dm644 "$srcdir/$pkgname.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"

  # Systemd service
  install -Dm644 "$srcdir/$pkgname.service" "$pkgdir/usr/lib/systemd/system/$pkgname.service"
}

post_install() {
  systemctl daemon-reload
  echo "Enable the IcingadDB service with: \"systemctl enable icingadb\""
}

pre_remove() {
  systemctl disable --now icingadb
}
