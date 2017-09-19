# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=5.1
pkgrel=1
pkgdesc="RapidDisk kernel modules (DKMS) and rapiddisk management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms' 'jansson')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($pkgname::git+https://github.com/pkoutoupis/rapiddisk.git
	)
sha256sums=('SKIP')

prepare() {
  cd "$srcdir/$pkgname"
  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$srcdir/$pkgname"/module/dkms.conf

  for filename in "${source[@]}"; do
    if [[ "$filename" =~ \.patch$ ]]; then
      msg2 "Applying patch $filename"
      patch --forward --strip=0 --input="$srcdir/$filename"
    fi
  done
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
