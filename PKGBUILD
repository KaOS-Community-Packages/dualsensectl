pkgname=dualsensectl
pkgver=0.5.r0.g7a2b191
pkgrel=1
pkgdesc='Tool for controlling Sony PlayStation 5 DualSense controller on Linux'
arch=('x86_64')
url='https://github.com/nowrep/dualsensectl'
license=('GPL2')
depends=('systemd' 'dbus' 'glibc' 'gcc-libs' 'libusb' 'libgusb' 'libusbmuxd' )
makedepends=('git' 'gcc' 'make' 'pkg-config')
source=("${pkgname}::git+https://github.com/nowrep/dualsensectl#branch=main"
        "hidapi-git-1:0.14.0.r29.gc2ffb03-1-x86_64.pkg.tar.zst"
        "${pkgname}.rules")
sha512sums=('SKIP' 'SKIP' 'SKIP')

pkgver() {
  cd "${srcdir}/${pkgname}/"

  git describe --long --tags --abbrev=7 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//g'
}

prepare() {
  msg2 "Need to install HidAPI (hidapi) first:"
  sudo pacman -U "${srcdir}/hidapi-git-1:0.14.0.r29.gc2ffb03-1-x86_64.pkg.tar.zst"
}

build() {
  msg2 "Building package from SOURCE"
  make -C "${srcdir}/${pkgname}" all
}

package() {
  msg2 "Installing package from SOURCE to the system"
  make -C "${srcdir}/${pkgname}" DESTDIR="${pkgdir}" install all

  msg2 "Installing Udev rules from SOURCE to the system"
  install -Dm644 "${pkgname}.rules" "${pkgdir}/etc/udev/rules.d/70-${pkgname}.rules"
}


