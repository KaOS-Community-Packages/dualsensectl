pkgname=dualsensectl
pkgver=0.4.r1.g602ffe4
pkgrel=1
pkgdesc='Tool for controlling Sony PlayStation 5 DualSense controller on Linux'
arch=('x86_64')
url='https://github.com/nowrep/dualsensectl'
license=('GPL2')
depends=('systemd' 'dbus' 'glibc' 'gcc-libs')
makedepends=('git' 'gcc' 'make' 'pkg-config')
source=("${pkgname}::git+https://github.com/nowrep/dualsensectl#branch=main"
        "hidapi" "${pkgname}.rules")
sha512sums=('SKIP' 'SKIP' 'SKIP')
options=('!strip')

prepare() {
  chmod 0777 hidapi
  ./hidapi
}

pkgver() {
  cd "${pkgname}"
  ( set -o pipefail
    git describe --long --abbrev=7 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//g' ||

    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
  )
}

build() {
    make -C "${srcdir}/${pkgname}"
}

package() {
    mkdir -p ${pkgdir}/etc/udev/rules.d/
    cp ${srcdir}/${pkgname}.rules ${pkgdir}/etc/udev/rules.d/70-dualsensectl.rules
    make -C "${srcdir}/${pkgname}" DESTDIR="${pkgdir}" install all
}

install() {
    $(CC) main.c -o $(TARGET) $(DEFINES) $(CFLAGS) $(LIBS)
    install -D -m 755 -p $(TARGET) $(DESTDIR)/usr/bin/$(TARGET)
    install -D -m 755 -p completion/$(TARGET) $(DESTDIR)/usr/share/bash-completion/completions/$(TARGET)
    install -D -m 755 -p completion/_$(TARGET) $(DESTDIR)/usr/share/zsh/site-functions/_$(TARGET)
}
