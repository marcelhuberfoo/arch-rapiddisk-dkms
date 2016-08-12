# Maintainer: Marcel Huber <`echo "moc tknup liamg tä oofrebuhlecram" | rev`>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=4.2
pkgrel=2
pkgdesc="RapidDisk kernel modules (DKMS) and rapiddisk management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($pkgname::git+http://git.rapiddisk.org/rapiddisk-4.x.git
        src_makefile.patch
        doc_makefile.patch
        module_flush_cache.patch)
sha256sums=('SKIP'
            '0c3bd26638203e5177564f87949dbadc96cdf8e6cf79a7bb2a5dc0bf6ea99f79'
            '116d3d9f71234cba9dce3cbbee94df0c717f087e0434c38309cb42b1dca9de88'
            'ffca1971e58220b7d3e7b3a8a8d287a81f7fd32c74dd28a3bfe582513e3e0860')

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
