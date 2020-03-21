# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org
# Contributor: Giovanni 'ItachiSan' Santini <giovannisantini93@yahoo.it>
# Contributor: Sven-Hendrik Haase <svenstaro@gmail.com>
# Contributor: hexchain <i@hexchain.org>

# Thanks Nicholas Guriev <guriev-ns@ya.ru> for the patches!
# https://github.com/mymedia2/tdesktop

pkgname=telegram-desktop-git
pkgver=1.9.21.r2.gd2291f5b1
pkgrel=1
pkgdesc='Official Telegram Desktop client - development branch'
arch=(x86_64)
options=(!buildflags)
url="https://desktop.telegram.org/"
license=('GPL3')
depends=( 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip'
          'openal' 'qt5-imageformats' 'xxhash'
)
makedepends=('cmake' 'git' 'microsoft-gsl' 'libdbusmenu-qt5'
             'ninja' 'python' 'range-v3' 'tl-expected'
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
    # "0005-Use-system-wide-fonts.patch"
)
sha512sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

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

    # patch -Np1 ../0005-Use-system-wide-fonts.patch
}

build() {
    cd "$srcdir/tdesktop"
    mkdir -p build
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
        -DTDESKTOP_USE_PACKAGED_TGVOIP=OFF \
        -DDESKTOP_APP_USE_PACKAGED_RLOTTIE=OFF \
        -DDESKTOP_APP_USE_PACKAGED_VARIANT=OFF \
        -DDESKTOP_APP_USE_PACKAGED_EXPECTED=ON \
        -DDESKTOP_APP_DISABLE_CRASH_REPORTS=ON \
        -DTDESKTOP_DISABLE_REGISTER_CUSTOM_SCHEME=ON \
        -DDESKTOP_APP_DISABLE_SPELLCHECK=ON \
        -DTDESKTOP_FORCE_GTK_FILE_DIALOG=ON \
        -DDESKTOP_APP_SPECIAL_TARGET=""
    ninja -j6 -C build
}

package() {
    cd "$srcdir/tdesktop"
    install -dm755 "$pkgdir/usr/bin"
    install -m755 "build/bin/telegram-desktop" "$pkgdir/usr/bin/telegram-desktop"

    install -d "$pkgdir/usr/share/applications"
    install -m644 "lib/xdg/telegramdesktop.desktop" "$pkgdir/usr/share/applications/telegramdesktop.desktop"

    install -d "$pkgdir/usr/share/metainfo/"
    install -m644 "lib/xdg/telegramdesktop.appdata.xml.in" "$pkgdir/usr/share/metainfo/telegramdesktop.appdata.xml.in"

    local icon_size icon_dir
    for icon_size in 16 32 48 64 128 256 512; do
        icon_dir="$pkgdir/usr/share/icons/hicolor/${icon_size}x${icon_size}/apps"

        install -d "$icon_dir"
        install -m644 "Telegram/Resources/art/icon${icon_size}.png" "$icon_dir/telegram.png"
    done
}
