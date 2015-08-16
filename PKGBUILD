# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Balló György <ballogyor+arch@gmail.com>

pkgname=indicator-datetime
pkgver=12.10.2
pkgrel=5
pkgdesc='A very, very simple clock'
arch=('i686' 'x86_64')
url="https://launchpad.net/${pkgname}"
license=('GPL')
depends=('evolution-data-server' 'geoclue' 'ido' 'libdbusmenu-gtk3' 'libindicator-gtk3')
makedepends=('intltool' 'python2')
options=('!emptydirs')
install="${pkgname}.install"
source=("${url}/${pkgver%.*}/${pkgver}/+download/${pkgname}-${pkgver}.tar.gz"
        "http://pkgbuild.com/~bgyorgy/sources/${pkgname}-translations-20130310.tar.gz"
        'systemd-support.patch')
sha256sums=('b32f272d218e4d0fad53c90433913e4a84e1c60031cb95c4d92d0b59f50e19d4'
            'e4e763e3ee9926841b208cc4652b1cd3c72626bd267dd2f589620ea9ceea511b'
            'd7b507e2e3bbc41dcb775c422d95042bb12cfe8418431ab965843554627fbda0')

prepare() {
  cd ${pkgname}-${pkgver}

  sed -i 's/-Werror//' */Makefile.{am,in}
  sed -i '/libedataserverui-3.0/d' configure{,.ac}
  patch -Np1 -i ../systemd-support.patch

# Install updated language files
  rename ${pkgname}- '' ../po/${pkgname}-*.po
  mv -f -t po ../po/*
  printf "%s\n" po/*.po | sed -e 's/po\///g' -e 's/\.po//g' >po/LINGUAS
}

build() {
  cd ${pkgname}-${pkgver}

  export CFLAGS="$CFLAGS -Wno-deprecated-declarations"
  autoreconf -fi
  ./configure --prefix='/usr' --sysconfdir='/etc' --localstatedir='/var' --libexecdir="/usr/lib/${pkgname}" --disable-{schemas-compile,static} --enable-systemd
  make
}

package() {
  cd ${pkgname}-${pkgver}

  make DESTDIR="${pkgdir}" install
}

# vim: ts=2 sw=2 et:
