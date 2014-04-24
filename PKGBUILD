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
  cd "$srcdir/$pkgname"
}

package() {
  install -Dm644 "$srcdir/$pkgname"/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"

  # Set name and version if variables were used/patched in dkms.conf
  sed -e "s/@_PKGBASE@/${_pkgbase}/" \
      -e "s/@PKGVER@/${pkgver}/" \
      -i "${pkgdir}"/usr/src/${_pkgbase}-${pkgver}/dkms.conf

  cp -r "$srcdir/$pkgname/module/drivers/block"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
