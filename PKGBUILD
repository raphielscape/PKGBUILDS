# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org>
# Contributor: Ricardo Liang (rliang) <ricardoliang@gmail.com>

_pkgname=mutter
pkgname="$_pkgname"-git
pkgver=3.35.92+5+g3c5547539
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
provides=(mutter)
conflicts=(mutter mutter-dev)
depends=(dconf js68 gjs-git gobject-introspection-runtime gsettings-desktop-schemas libcanberra
         startup-notification zenity libsm gnome-desktop upower libxkbcommon-x11
         gnome-settings-daemon libgudev libinput pipewire-git sysprof-git)
makedepends=(intltool gobject-introspection git egl-wayland)
groups=(gnome)
source=("git+https://gitlab.gnome.org/GNOME/mutter.git")
sha256sums=('SKIP')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $srcdir/mutter
}

build() {
  CFLAGS+=" -O3 -falign-functions=32 -flto=thin \
            -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong -funroll-loops"

  CXXFLAGS+=" -O3 -falign-functions=32 -flto=thin \
              -fno-math-errno -fno-trapping-math \
              -fstack-protector-strong -funroll-loops"

  arch-meson $_pkgname build
  ninja -C build
}

package() {
  DESTDIR="$pkgdir" meson install -C build
}
