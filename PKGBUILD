# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Ionut Biru <ibiru@archlinux.org>

_pkgname=gjs
pkgname=$_pkgname-git
pkgbase=$_pkgname-git
pkgver=1.63.92+1+g351ca384
pkgrel=1
epoch=2
pkgdesc="Javascript Bindings for GNOME"
url="https://gitlab.gnome.org/GNOME/gjs/blob/master/doc/Home.md"
arch=(x86_64)
license=(GPL)
depends=('cairo' 'gobject-introspection-runtime' 'js68' 'dconf')
makedepends=('gobject-introspection' 'git' 'autoconf-archive' 'sysprof' 'meson')
checkdepends=('valgrind' 'xorg-server-xvfb')
conflicts=($_pkgname)
provides=($_pkgname=$pkgver)
source=("git+https://gitlab.gnome.org/GNOME/gjs.git")
sha256sums=('SKIP')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/-/+/g'
}

build() {
  CFLAGS+=" -O3 -falign-functions=32 -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong -funroll-loops"
  CXXFLAGS+=" -O3 -falign-functions=32 -fno-math-errno -fno-trapping-math \
              -fstack-protector-strong -funroll-loops"

  arch-meson $_pkgname build
  ninja -C build
}

check() {
  meson test -C build --print-errorlogs
}

package() {
  DESTDIR="$pkgdir" meson install -C build
}
