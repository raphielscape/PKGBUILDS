# Maintainer: Raphiel Rollerscaperers <raphielscape@raphielgang.org
# Contributor: Giovanni 'ItachiSan' Santini <giovannisantini93@yahoo.it>
# Contributor: Sven-Hendrik Haase <svenstaro@gmail.com>
# Contributor: hexchain <i@hexchain.org>

# Thanks Nicholas Guriev <guriev-ns@ya.ru> for the patches!
# https://github.com/mymedia2/tdesktop

pkgname=telegram-desktop-git
pkgver=2.1.12.r0.ga416debc2
pkgrel=1
pkgdesc='Official Telegram Desktop client - development branch'
arch=(x86_64)
url="https://desktop.telegram.org/"
license=('GPL3')
options=(!buildflags)
depends=( 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip'
          'openal' 'qt5-imageformats' 'xxhash' 'enchant'
)
makedepends=('cmake' 'git' 'microsoft-gsl' 'libdbusmenu-qt5'
             'ninja' 'python' 'range-v3' 'libappindicator-gtk3'
)
optdepends=(
    'ttf-opensans: default Open Sans font family'
    'libdbusmenu-qt5: For Notifications in QT5'
    'libappindicator-gtk3: For GTK3 Notification'
)
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=("tdesktop::git+https://github.com/telegramdesktop/tdesktop.git#branch=dev"
        "libtgvoip::git+https://github.com/telegramdesktop/libtgvoip"
        "variant::git+https://github.com/desktop-app/variant.git#branch=desktop-app"
        "GSL::git+https://github.com/Microsoft/GSL.git"
        "Catch::git+https://github.com/philsquared/Catch"
        "xxHash::git+https://github.com/Cyan4973/xxHash.git"
        "rlottie::git+https://github.com/desktop-app/rlottie.git"
        "lz4::git+https://github.com/lz4/lz4.git"
        "lib_crl::git+https://github.com/desktop-app/lib_crl.git"
        "lib_rpl::git+https://github.com/desktop-app/lib_rpl.git"
        "lib_base::git+https://github.com/desktop-app/lib_base.git"
        "codegen::git+https://github.com/desktop-app/codegen.git"
        "lib_ui::git+https://github.com/desktop-app/lib_ui.git"
        "lib_rlottie::git+https://github.com/desktop-app/lib_rlottie.git"
        "lib_lottie::git+https://github.com/desktop-app/lib_lottie.git"
        "lib_tl::git+https://github.com/desktop-app/lib_tl.git"
        "lib_spellcheck::git+https://github.com/desktop-app/lib_spellcheck"
        "lib_storage::git+https://github.com/desktop-app/lib_storage.git"
        "cmake_helpers::git+https://github.com/desktop-app/cmake_helpers.git"
        "expected::git+https://github.com/TartanLlama/expected"
        "tl-cmake::git+https://github.com/TartanLlama/tl-cmake.git"
        "QR-Code-generator::git+https://github.com/nayuki/QR-Code-generator"
        "lib_qr::git+https://github.com/desktop-app/lib_qr.git"
        "libdbusmenu-qt::git+https://github.com/desktop-app/libdbusmenu-qt.git"
        "materialdecoration::git+https://github.com/desktop-app/materialdecoration.git"
)
sha512sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
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
    git config submodule.Telegram/ThirdParty/libtgvoip.url "$srcdir/libtgvoip"
    git config submodule.Telegram/ThirdParty/variant.url "$srcdir/variant"
    git config submodule.Telegram/ThirdParty/GSL.url "$srcdir/GSL"
    git config submodule.Telegram/ThirdParty/Catch.url "$srcdir/Catch"
    git config submodule.Telegram/ThirdParty/xxHash.url "$srcdir/xxHash"
    git config submodule.Telegram/ThirdParty/rlottie.url "$srcdir/rlottie"
    git config submodule.Telegram/ThirdParty/lz4.url "$srcdir/lz4"
    git config submodule.Telegram/lib_crl.url "$srcdir/lib_crl"
    git config submodule.Telegram/lib_rpl.url "$srcdir/lib_rpl"
    git config submodule.Telegram/lib_base.url "$srcdir/lib_base"
    git config submodule.Telegram/codegen.url "$srcdir/codegen"
    git config submodule.Telegram/lib_ui.url "$srcdir/lib_ui"
    git config submodule.Telegram/lib_rlottie.url "$srcdir/lib_rlottie"
    git config submodule.Telegram/lib_lottie.url "$srcdir/lib_lottie"
    git config submodule.Telegram/lib_tl.url "$srcdir/lib_tl"
    git config submodule.Telegram/lib_spellcheck.url "$srcdir/lib_spellcheck"
    git config submodule.Telegram/lib_storage.url "$srcdir/lib_storage"
    git config submodule.cmake.url "$srcdir/cmake_helpers"
    git config submodule.Telegram/ThirdParty/expected.url "$srcdir/expected"
    git config submodule.Telegram/ThirdParty/QR.url "$srcdir/QR-Code-generator"
    git config submodule.Telegram/lib_qr.url "$srcdir/lib_qr"
    git config submodule.Telegram/ThirdParty/libdbusmenu-qt.url "$srcdir/libdbusmenu-qt"
    git config submodule.Telegram/ThirdParty/materialdecoration.url "$srcdir/materialdecoration"
    git submodule update --recursive
}

build() {
    cd "$srcdir/tdesktop"
    CFLAGS+="-falign-functions=32 -fno-math-errno -fno-trapping-math \
            -fstack-protector-strong -funroll-loops"
    CXXFLAGS+="-march=native -mtune=native -falign-functions=32 -fno-math-errno -Wno-unknown-warning-option\
              -fno-trapping-math -fstack-protector-strong -funroll-loops -pipe -fno-plt \
              -fasynchronous-unwind-tables -fno-omit-frame-pointer -Wp,-D_REENTRANT"
    LDFLAGS+="-Wl,-O2,--sort-common,--as-needed,-z,relro,-z,now"
    cmake -B build -G Ninja . \
        -DCMAKE_INSTALL_PREFIX="/usr" \
        -DCMAKE_BUILD_TYPE=Release \
        -DTDESKTOP_API_ID=17349 \
        -DTDESKTOP_API_HASH=344583e45741c457fe1862106095a5eb \
        -DDESKTOP_APP_USE_PACKAGED=ON \
        -DDESKTOP_APP_USE_PACKAGED_FONTS=ON \
        -DDESKTOP_APP_USE_PACKAGED_RLOTTIE=OFF \
        -DDESKTOP_APP_USE_PACKAGED_VARIANT=OFF \
        -DDESKTOP_APP_USE_PACKAGED_EXPECTED=OFF \
        -DDESKTOP_APP_USE_PACKAGED_GSL=OFF \
        -DTDESKTOP_DISABLE_REGISTER_CUSTOM_SCHEME=ON \
        -DTDESKTOP_USE_PACKAGED_TGVOIP=OFF \
        -DDESKTOP_APP_DISABLE_CRASH_REPORTS=ON \
        -DTDESKTOP_USE_FONTCONFIG_FALLBACK=OFF \
        -DTDESKTOP_LAUNCHER_BASENAME="telegramdesktop"
    ninja -j6 -C build
}

package() {
    cd "$srcdir/tdesktop"
    DESTDIR=$pkgdir ninja -C build install
}
