# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=2.12
pkgrel=3
pkgdesc="RapidDisk kernel modules (DKMS) and rxadm management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($pkgname::git+http://git.rapiddisk.org/rxdsk-2.x.git
        pyRxAdm.patch
        cmd_makefile.patch
        man_makefile.patch)
sha256sums=('SKIP'
            'e1475b8800e279885598839655aa7fed89bb1ea6554ffa22faacccf93eb72866'
            '358c28a32bffa278618858893e2ee64be1e09b7ff780b33f02f239edf8ece644'
            '2cdcbf37926e9aa86e74424275ee66dd4eb84ef73cf2b9287bdf570d64addba9')

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
  patch --forward --strip=0 --input="$srcdir/man_makefile.patch"
  patch --forward --strip=0 --input="$srcdir/cmd_makefile.patch"
}

build() {
  cd "$srcdir/$pkgname"
  # build rxadm utility
  make SUBDIRS="cmd" all
}

package() {
  # install man page
  cd "$srcdir/$pkgname"
  make SUBDIRS="man" DESTDIR="$pkgdir" install
  make SUBDIRS="cmd" DESTDIR="$pkgdir" BIN_DIR="/usr/bin" ADM_DIR="/usr/bin" LOGO_DIR="/usr/share/pixmaps" install
  # copy kernel module source files for dkms use
  install -Dm644 "$srcdir/$pkgname"/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"
  mkdir -p "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
  install -Dm644 "$srcdir/$pkgname/module/drivers/block"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
