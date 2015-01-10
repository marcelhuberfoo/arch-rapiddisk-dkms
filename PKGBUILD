# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=2.12
pkgrel=2
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
            '249812a4a110dfc5ef92ea5a68278e375dc3d95606b32bf315bee84b61ca986a')

prepare() {
  cd "$srcdir/$pkgname"
  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$srcdir/$pkgname"/dkms.conf
  # modify icon location within python tool
  patch --forward --strip=0 --input="$srcdir/pyRxAdm.patch"
  sed -r -e "1 s|python$|python2|" \
      -i "$srcdir/$pkgname"/cmd/pyRxAdm.py
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
  mkdir -p "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
  install -Dm644 "$srcdir/$pkgname/module/drivers/block"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
