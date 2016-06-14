# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=3.7
pkgrel=1
pkgdesc="RapidDisk kernel modules (DKMS) and rapiddisk management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($pkgname::git+http://git.rapiddisk.org/rxdsk-3.x.git
        src_makefile.patch
        doc_makefile.patch)
sha256sums=('SKIP'
            '9a79ca81d12ad6b67bb7db1836ad2c811a28103ab56945da95f384458b81c2e4'
            '7d0b03846128e734b0265b047d2317852d1400d7e832dd4f1a3b9d880462ecda')

prepare() {
  cd "$srcdir/$pkgname"
  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$srcdir/$pkgname"/module/dkms.conf
  patch --forward --strip=0 --input="$srcdir/doc_makefile.patch"
  patch --forward --strip=0 --input="$srcdir/src_makefile.patch"
}

build() {
  cd "$srcdir/$pkgname"
  # build rapiddisk utility
  make SUBDIRS="src" all
}

package() {
  # install man page
  cd "$srcdir/$pkgname"
  make SUBDIRS="doc" DESTDIR="$pkgdir" install
  make SUBDIRS="src" DESTDIR="$pkgdir" DIR="/usr/bin" install
  # copy kernel module source files for dkms use
  install -Dm644 "$srcdir/$pkgname"/module/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"
  mkdir -p "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
  install -Dm644 "$srcdir/$pkgname/module/"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
