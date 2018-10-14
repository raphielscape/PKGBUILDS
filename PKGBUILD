# Maintainer: Raphiel Rollerscaperers <raphielscape@outlook.com>
# Contributor: Manuel Hüsers <manuel.huesers@uni-ol.de>
# Contributor: Iwan Timmer <irtimmer@gmail.com>
# Contributor: Timothée Ravier <tim at siosm dot fr>
# Contributor: Tom <reztho at archlinux dot org>

pkgname=tuned-git
pkgver=2.10.0
pkgrel=3
pkgdesc='Daemon that performs monitoring and adaptive configuration of devices in the system'
arch=('any')
url="https://github.com/redhat-performance/tuned}"
license=('GPL')
depends=('ethtool' 'python-configobj' 'python-pyudev' 'python-gobject2' 'python-decorator' 'python-dbus' 
'python-gobject' 'python-linux-procfs' 'dbus-glib')
optdepends=('virt-what: For use with virtual machines' 'systemtap: Disk and net statistic monitoring systemtap scripts')
makedepends=('desktop-file-utils')
backup=('etc/tuned/active_profile')
install="tuned-git.install"
source=("https://github.com/redhat-performance/tuned/archive/master.zip")
sha256sums=('3b675a33e3d11563db49ee49ca16e1b918d11e81908fe50413e8d86d6acfe56f')

prepare() {
  unzip -u master.zip
}

package() {
	cd "tuned-master"

	make DESTDIR="${pkgdir}" install

	mv "${pkgdir}"/usr/sbin/* "${pkgdir}"/usr/bin/
	mv "${pkgdir}"/usr/libexec/tuned/* "${pkgdir}"/usr/lib/tuned/
	rm -r "${pkgdir}"/run "${pkgdir}"/usr/sbin "${pkgdir}"/usr/libexec

	find "${pkgdir}"/usr/bin/ -type f -exec sed -i 's@#!/usr/bin/python23@#!/usr/bin/python2@' {} \;

	install -Dm644 "${srcdir}/tuned-master/tuned.service" "${pkgdir}/usr/lib/systemd/system/tuned.service"
}
