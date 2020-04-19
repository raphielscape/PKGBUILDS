# Maintainer: Raphiel Rollerscaperers <rapherion@raphielgang.org>
# Contributor: Jianqiu Zhang <void001@archlinuxcn.org>

pkgname=oomd-git
_pkgname=oomd
pkgver=v0.3.2+12+g51553b5
pkgrel=1
pkgdesc='A userspace out-of-memory killer'
arch=('x86_64')
url="https://github.com/facebookincubator/oomd"
license=('GPL2')
depends=('jsoncpp')
optdepends=('systemd-libs')
makedepends=('meson' 'ninja')
checkdepends=('gtest' 'gmock')
backup=("etc/${_pkgname}/${_pkgname}.json")
install="${_pkgname}.install"
md5sums=('SKIP')

source=(
    "oomd::git+https://github.com/facebookincubator/oomd.git"
)

pkgver() {
    cd "$srcdir/$_pkgname"
    git describe --tags | sed 's/-/+/g'
}

build() {
    cd "$srcdir/$_pkgname"
    meson --prefix "/usr" build && ninja -C build
}

check() {
    cd "$srcdir/$_pkgname"
    ninja test -C build
}

package() {
    cd "$srcdir/$_pkgname"
    DESTDIR="$pkgdir" ninja -C build install

    install -d "$pkgdir/etc/"
    install -m644 "$srcdir/oomd/src/oomd/etc/desktop.json" "$pkgdir/etc/desktop.json.example"
}
