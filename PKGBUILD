# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Ionut Biru <ibiru@archlinux.org>

_pkgname=gjs
pkgname=$_pkgname-git
pkgbase=$_pkgname-git
pkgver=1.63.1+1+g466678d9
pkgrel=1
epoch=2
pkgdesc="Javascript Bindings for GNOME"
url="https://wiki.gnome.org/Projects/Gjs"
arch=(x86_64)
license=(GPL)
conflicts=($_pkgname)
provides=($_pkgname=$pkgver)
depends=(cairo gobject-introspection-runtime js60 dconf)
makedepends=(gobject-introspection git autoconf-archive sysprof)
checkdepends=(valgrind xorg-server-xvfb)
source=("git+https://gitlab.gnome.org/GNOME/gjs.git")
sha256sums=('SKIP')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $_pkgname
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $_pkgname

  CFLAGS+=" -O3 -falign-functions=32 -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong"
  CXXFLAGS+=" -O3 -falign-functions=32 -fno-math-errno -fno-trapping-math \
              -fstack-protector-strong"

  ./configure \
    --prefix=/usr \
    --libexecdir=/usr/lib \
    --disable-static \
    --enable-compile-warnings=yes
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

check() {
  cd $_pkgname
  xvfb-run make check
}

package() {
  cd $_pkgname
  make DESTDIR="$pkgdir" install
}