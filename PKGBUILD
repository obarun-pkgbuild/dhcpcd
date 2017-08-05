# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/dhcpcd
# 						Maintainer: Ronald van Haren <ronald.archlinux.org>
# 						Contributor: Tom Killian <tom.archlinux.org>
# 						Contributor: Judd Vinet <jvinet.zeroflux.org>

pkgname=dhcpcd
pkgver=6.11.5
pkgrel=1
pkgdesc="RFC2131 compliant DHCP client daemon"
url="http://roy.marples.name/projects/dhcpcd/"
arch=('x86_64')
license=('BSD')
groups=('base')
depends=('glibc' 'sh' 'udev')
optdepends=('openresolv: resolvconf support'
			'dhcpcd-s6serv: S6 dhcpcd service'
			'dhcpcd-s6rcserv: S6-rc dhcpcd service'
			'dhcpcd-runitserv: dhcpcd runit service')
provides=('dhcp-client')
backup=('etc/dhcpcd.conf')
options=('emptydirs')  # We Need the Empty /var/lib/dhcpcd Directory
source=("http://roy.marples.name/downloads/${pkgname}/${pkgname}-$pkgver.tar.xz"
        dhcpcd.conf)
sha1sums=('6e362f8fcb7e39759ede1ae0f4f98d29f59eb487'
          '341e1ba3b10367dad181403f112e110a47a19aa6')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal


build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  # configure variables
  ./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--localestatdir=/var \
		--sbindir=/usr/bin \
		--libexecdir=/usr/lib/dhcpcd \
		--dbdir=/var/lib/dhcpcd \
		--rundir=/run \
		
  # Build
  make
}

check() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  make test
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" install

  # Install License
  install -d "$pkgdir/usr/share/licenses/${pkgname}"
  sed 26q "$srcdir/${pkgname}-$pkgver/control.h" \
      >>"$pkgdir/usr/share/licenses/${pkgname}/LICENSE"

  # Set Options in /etc/dhcpcd.conf
  install -m644 "${srcdir}/dhcpcd.conf" "${pkgdir}/etc/dhcpcd.conf"

}
