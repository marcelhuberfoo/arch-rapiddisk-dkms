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
source=($pkgname::git+http://git.rapiddisk.org/rxdsk-2.x.git
        pyRxAdm.patch)
sha256sums=('SKIP'
            '4e8734a042d3f51d0ac7c7301490b05b9d5a5c24512df88efa3b08acf3e967d5')

prepare() {
  cd "$srcdir/$pkgname"
}

build() {
  # build rxadm utility
  make -C "$srcdir/$pkgname/cmd"
}

package() {
  install -Dm755 "$srcdir/$pkgname"/cmd/rxadm "$pkgdir/usr/bin/rxadm"
  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$srcdir/$pkgname"/dkms.conf
  # modify icon location within python tool
  patch -Np0 -i "$srcdir/pyRxAdm.patch"
  sed -r -e "1 s|python$|python2|" \
      -i "$srcdir/$pkgname"/pyRxAdm.py
  # optionally copy pyRxAdm.py and icon
  install -Dm755 "$srcdir/$pkgname"/cmd/pyRxAdm.py "$pkgdir/usr/bin/pyRxAdm.py"
  install -Dm444 "$srcdir/$pkgname"/misc/rxadm_logo_48x48.png "$pkgdir/usr/share/pixmaps/rxadm.png"
  install -Dm644 "$srcdir/$pkgname"/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"


  cp -r "$srcdir/$pkgname/module/drivers/block"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
