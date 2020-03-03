# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org
# Contributor: Giovanni 'ItachiSan' Santini <giovannisantini93@yahoo.it>
# Contributor: Sven-Hendrik Haase <svenstaro@gmail.com>
# Contributor: hexchain <i@hexchain.org>

# Thanks Nicholas Guriev <guriev-ns@ya.ru> for the patches!
# https://github.com/mymedia2/tdesktop

pkgname=telegram-desktop-git
pkgver=1.9.19.r9.g4e345ac68
pkgrel=1
pkgdesc='Official Telegram Desktop client - development branch'
arch=(x86_64)
options=(!buildflags)
url="https://desktop.telegram.org/"
license=('GPL3')
depends=( 'enchant' 'ffmpeg' 'hicolor-icon-theme' 'lz4'
          'minizip' 'openal' 'qt5-imageformats' 'xxhash'
)
makedepends=('cmake' 'git' 'microsoft-gsl' 'libdbusmenu-qt5'
             'ninja' 'python' 'quilt' 'range-v3'
)
optdepends=(
    'ttf-opensans: default Open Sans font family'
    'libdbusmenu-qt5: For Notifications in QT5'
)
provides=('telegram-desktop')
conflicts=('telegram-desktop')
source=(
    # Git repositories; might be adjusted when a build issue arise.
    "tdesktop::git+https://github.com/telegramdesktop/tdesktop.git#branch=dev"
    "Catch2::git+https://github.com/catchorg/Catch2.git"
    "crl::git+https://github.com/telegramdesktop/crl.git"
    "GSL::git+https://github.com/Microsoft/GSL.git"
    "libtgvoip::git+https://github.com/telegramdesktop/libtgvoip.git"
    "rlottie::git+https://github.com/desktop-app/rlottie.git"
    "variant::git+https://github.com/mapbox/variant.git"
    "xxHash::git+https://github.com/Cyan4973/xxHash.git"
    # These files might require modifications to be up-to-date. If that is the
    # case, they will be updated in place and untracked temporarily.
    "0005-Use-system-wide-fonts.patch"
    "series"
)
sha512sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'd0327d992d939a72dcfd3b35ac84931c08b67e189978cd65cb9a6cd4dc868334c9a13c773cba7882f429ce62c2631d1c70e0600d04e24353ebb76feb09084f65'
            'd66adb920b318520146cf7a29f13e28e16c0cfb28f4d27ef743a8d7acea4b45b5c62e7dfc3928ec23a97b12dd42845f311aaa7c317df632f7926bdd5bfb8f2b3')

pkgver() {
    cd "$srcdir/tdesktop"
    git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "$srcdir/tdesktop"
    git submodule init
    git config submodule.Telegram/ThirdParty/Catch.url "$srcdir/Catch2"
    git config submodule.Telegram/ThirdParty/crl.url "$srcdir/crl"
    git config submodule.Telegram/ThirdParty/GSL.url "$srcdir/GSL"
    git config submodule.Telegram/ThirdParty/libtgvoip.url "$srcdir/libtgvoip"
    git config submodule.Telegram/ThirdParty/rlottie.url "$srcdir/rlottie"
    git config submodule.Telegram/ThirdParty/variant.url "$srcdir/variant"
    git config submodule.Telegram/ThirdParty/xxHash.url "$srcdir/xxHash"
    git submodule update

    QUILT_PATCHES=.. quilt --quiltrc=/dev/null push -a
}

build() {
    cd "$srcdir/tdesktop"
    mkdir build
    export CXXFLAGS="$CXXFLAGS -march=native -mtune=native \
                    -pipe -fno-plt -fasynchronous-unwind-tables \
                    -fno-omit-frame-pointer -Wp,-D_REENTRANT \
                    -ffile-prefix-map=$srcdir/tdesktop="
    cmake -B build -G Ninja . \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=Release \
        -DTDESKTOP_API_ID=17349 \
        -DTDESKTOP_API_HASH=344583e45741c457fe1862106095a5eb \
        -DDESKTOP_APP_USE_GLIBC_WRAPS=OFF \
        -DDESKTOP_APP_USE_PACKAGED=ON \
        -DDESKTOP_APP_USE_PACKAGED_FONTS=ON \
        -DDESKTOP_APP_USE_PACKAGED_RLOTTIE=OFF \
        -DTDESKTOP_USE_PACKAGED_TGVOIP=OFF \
        -DDESKTOP_APP_USE_PACKAGED_EXPECTED=OFF \
        -DDESKTOP_APP_USE_PACKAGED_VARIANT=OFF \
        -DDESKTOP_APP_DISABLE_CRASH_REPORTS=ON \
        -DTDESKTOP_DISABLE_AUTOUPDATE=ON \
        -DTDESKTOP_DISABLE_REGISTER_CUSTOM_SCHEME=ON \
        -DTDESKTOP_DISABLE_DESKTOP_FILE_GENERATION=ON \
        -DDESKTOP_APP_SPECIAL_TARGET=""
    ninja -C build
}

package() {
    cd "$srcdir/tdesktop"
    install -dm755 "$pkgdir/usr/bin"
    install -m755 "build/bin/telegram-desktop" "$pkgdir/usr/bin/telegram-desktop"

    install -d "$pkgdir/usr/share/applications"
    install -m644 "lib/xdg/telegramdesktop.desktop" "$pkgdir/usr/share/applications/telegramdesktop.desktop"

    install -d "$pkgdir/usr/share/metainfo/"
    install -m644 "lib/xdg/telegramdesktop.appdata.xml" "$pkgdir/usr/share/metainfo/telegramdesktop.appdata.xml"

    local icon_size icon_dir
    for icon_size in 16 32 48 64 128 256 512; do
        icon_dir="$pkgdir/usr/share/icons/hicolor/${icon_size}x${icon_size}/apps"

        install -d "$icon_dir"
        install -m644 "Telegram/Resources/art/icon${icon_size}.png" "$icon_dir/telegram.png"
    done
}
