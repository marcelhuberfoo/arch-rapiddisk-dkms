# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=2.12
pkgrel=1
pkgdesc="RapidDisk kernel modules (DKMS) and rxadm management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($pkgname::git+http://git.rapiddisk.org/rxdsk-2.x.git)
sha256sums=('SKIP')

prepare() {
  cd "$srcdir/$pkgname"
}

build() {
  # build rxadm utility
  make -C "$srcdir/$pkgname/cmd"
}

package() {
  install -Dm755 "$srcdir/$pkgname"/cmd/rxadm "$pkgdir/usr/bin/rxadm"
  # optionally copy pyRxAdm.py and icon
  install -Dm755 "$srcdir/$pkgname"/cmd/pyRxAdm.py "$pkgdir/usr/bin/pyRxAdm.py"
  install -Dm444 "$srcdir/$pkgname"/misc/rxadm_logo_48x48.png "$pkgdir/usr/share/pixmaps/rxadm.png"
  install -Dm644 "$srcdir/$pkgname"/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"

  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"
  # modify icon location within python tool
  sed -r -e "s|/opt/rxadm/rxadm_logo_48x48.png|/usr/share/pixmaps/rxadm.png|" \
      -i "$pkgdir/usr/bin/pyRxAdm.py"

  cp -r "$srcdir/$pkgname/module/drivers/block"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
