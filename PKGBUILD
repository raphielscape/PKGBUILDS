# Maintainer: Raphiel Rollerscaperers <raphielscape@outlook.com>
# Contributor: Manuel Hüsers <manuel.huesers@uni-ol.de>
# Contributor: Iwan Timmer <irtimmer@gmail.com>
# Contributor: Timothée Ravier <tim at siosm dot fr>
# Contributor: Tom <reztho at archlinux dot org>

pkgname=tuned-git
pkgver=2.14.0.r27.g845bdc5
pkgrel=1
pkgdesc='Daemon that performs monitoring and adaptive configuration of devices in the system'
arch=('any')
url="https://github.com/redhat-performance/tuned"
license=('GPL')
depends=('ethtool' 'python-configobj' 'python-pyudev' 'python-gobject' 'python-decorator' 'python-dbus'
'python-gobject' 'python-linux-procfs' 'dbus-glib')
optdepends=('virt-what: For use with virtual machines' 'systemtap: Disk and net statistic monitoring systemtap scripts' 'python-dmidecode: DMI Decoding stuff')
makedepends=('desktop-file-utils')
backup=('etc/tuned/active_profile')
install="tuned-git.install"
source=("git+https://github.com/redhat-performance/tuned.git")
sha256sums=('SKIP')

pkgver() {
  cd "tuned"
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
	cd "tuned"

	make DESTDIR="${pkgdir}" install

	mv "${pkgdir}"/usr/sbin/* "${pkgdir}"/usr/bin/
	mv "${pkgdir}"/usr/libexec/tuned/* "${pkgdir}"/usr/lib/tuned/
	rm -r "${pkgdir}"/run "${pkgdir}"/usr/sbin "${pkgdir}"/usr/libexec

	# Arch Linux doesn't have python2.3, change it to python2 instead to use python2.7
	find "${pkgdir}"/usr/bin/ -type f -exec sed -i 's@#!/usr/bin/python23@#!/usr/bin/python2@' {} \;

	install -Dm644 "${srcdir}/tuned/tuned.service" "${pkgdir}/usr/lib/systemd/system/tuned.service"
}
