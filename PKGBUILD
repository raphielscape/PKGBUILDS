# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org>
# Contributor: Ricardo Liang (rliang) <ricardoliang@gmail.com>

_pkgname=mutter
pkgname="$_pkgname"-git
pkgver=3.35.1
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://gitlab.gnome.org/GNOME/mutter"
arch=(x86_64)
license=(GPL)
provides=(mutter)
conflicts=(mutter mutter-dev)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas-git libcanberra
         startup-notification zenity libsm gnome-desktop upower libxkbcommon-x11
         gnome-settings-daemon libgudev libinput pipewire)
makedepends=(intltool gobject-introspection git egl-wayland meson)
groups=(gnome)
source=("git+https://gitlab.gnome.org/GNOME/mutter.git"
        0001-meta-profiler-Fix-compilation.patch)
sha256sums=('SKIP'
            '5bbad5c7a34dd2a575361a9b1f4622bf26771ef2aa09b6aa117750336d809001')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $srcdir/mutter
  # Fix build breakage
  patch -Np1 -i ../0001-meta-profiler-Fix-compilation.patch
}

build() {
  CFLAGS+=" -O3 -falign-functions=32 -flto=thin \
            -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong"

  CXXFLAGS+=" -O3 -falign-functions=32 -flto=thin \
              -fno-math-errno -fno-trapping-math \
              -fstack-protector-strong"

  arch-meson $_pkgname build
  ninja -C build
}

package() {
  DESTDIR="$pkgdir" meson install -C build
}
