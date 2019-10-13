# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org>
# Contributor: Ricardo Liang (rliang) <ricardoliang@gmail.com>

_pkgname=gnome-shell
pkgname="$_pkgname"-git
pkgver=3.35.1
pkgrel=1
epoch=1
pkgdesc="Next generation desktop shell"
url="https://gitlab.gnome.org/GNOME/gnome-shell"
arch=(x86_64)
license=(GPL2)
provides=(gnome-shell)
conflicts=(gnome-shell gnome-shell-dev)
depends=(accountsservice gcr gjs gnome-bluetooth upower gnome-session gnome-settings-daemon
         gnome-themes-extra gsettings-desktop-schemas libcanberra-pulse libcroco libgdm libsecret
         mutter-git nm-connection-editor unzip gstreamer libibus)
makedepends=(gtk-doc gnome-control-center evolution-data-server gobject-introspection git meson
             sassc asciidoc)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
groups=(gnome)
source=("git+https://gitlab.gnome.org/GNOME/gnome-shell.git"
        "git+https://gitlab.gnome.org/GNOME/libgnome-volume-control.git")
sha256sums=('SKIP'
            'SKIP')

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $_pkgname

  # Move the plugin to our custom epiphany-only dir
  sed -i "s/'mozilla'/'epiphany'/g" meson.build

  git submodule init
  git config --local submodule.subprojects/gvc.url "$srcdir/libgnome-volume-control"
  git submodule update
}

build() {
  CFLAGS+=" -O3 -falign-functions=32 -flto=thin \
            -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong"

  CXXFLAGS+=" -O3 -falign-functions=32 -flto=thin \
            -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong"

  LDFLAGS+=" -flto=thin"

  arch-meson $_pkgname build -D gtk_doc=true -Dnetworkmanager=true -Dsystemd=true
  ninja -C build
}

package() {
  DESTDIR="$pkgdir" meson install -C build

  # https://bugs.archlinux.org/task/37412
  mkdir "$pkgdir/usr/share/gnome-shell/modes"
}
