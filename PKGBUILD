# Maintainer: Marcel Huber <marcelhuberfoo at gmail dot com>

_pkgbase=rapiddisk
pkgname=rapiddisk-dkms
pkgver=5.2.2.gb7a05f1
pkgrel=1
pkgdesc="RapidDisk kernel modules (DKMS) and rapiddisk management utility"
arch=('i686' 'x86_64')
url="http://www.rapiddisk.org/"
license=('GPL2')
depends=('dkms' 'jansson')
conflicts=("$_pkgbase")
install=$pkgname.install
source=($_pkgbase::git+https://github.com/pkoutoupis/rapiddisk.git
	)
sha256sums=('SKIP')

pkgver() {
  cd "$srcdir/$_pkgbase" 2>/dev/null && (
  # update dependent projects revision information
  if GITTAG="$(git describe --abbrev=0 --tags 2>/dev/null)"; then
    local _revs_ahead_tag=$(git rev-list --count ${GITTAG}..)
    local _commit_id_short=$(git log -1 --format=%h)
    echo $(sed -e s/^${pkgbase%%-git}// -e 's/^[-_/a-zA-Z]\+//' -e 's/[-_+]/./g' <<< ${GITTAG}).${_revs_ahead_tag}.g${_commit_id_short}
  else
    echo 0.$(git rev-list --count master).g$(git log -1 --format=%h)
  fi
  ) || echo $pkgver
}

prepare() {
  cd "$srcdir/$_pkgbase"
  # ensure correct name and version in dkms.conf
  sed -r -e "s/^(PACKAGE_NAME=).*$/\1\"${_pkgbase}\"/" \
      -e "s/^(PACKAGE_VERSION=).*$/\1\"${pkgver}\"/" \
      -i "$srcdir/$_pkgbase"/module/dkms.conf

  for filename in "${source[@]}"; do
    if [[ "$filename" =~ \.patch$ ]]; then
      msg2 "Applying patch $filename"
      patch --forward --strip=0 --input="$srcdir/$filename"
    fi
  done
}

build() {
  cd "$srcdir/$_pkgbase"
  # build rapiddisk utility
  make SUBDIRS="src" all
}

package() {
  # install man page
  cd "$srcdir/$_pkgbase"
  make SUBDIRS="doc" DESTDIR="$pkgdir" install
  make SUBDIRS="src" DESTDIR="$pkgdir" DIR="/usr/bin" install
  # copy kernel module source files for dkms use
  install -Dm644 "$srcdir/$_pkgbase"/module/dkms.conf "$pkgdir/usr/src/${_pkgbase}-${pkgver}/dkms.conf"
  mkdir -p "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
  install -Dm644 "$srcdir/$_pkgbase/module/"/* "$pkgdir/usr/src/${_pkgbase}-${pkgver}/"
}
