# Maintainer: Stefan Zipproth <s.zipproth@ditana.org>

pkgname=ditana-network
pkgver=1.07
pkgrel=1
pkgdesc="Ditana network configuration"
arch=(any)
url="https://ditana.org"
license=('AGPL-3.0-or-later AND LGPL-2.1-or-later AND GPL-2.0-or-later AND GPL-3.0-or-later AND CC0-1.0 AND MIT-0')
conflicts=()
depends=(networkmanager inetutils systemd)
makedepends=()
backup+=(
    'etc/NetworkManager/conf.d/dns.conf'
)
source=(
    'dns.conf'
    '80-ditana.conf'
    '90-ditana.conf'
)
sha256sums=(
    'SKIP'
    'SKIP'
    'SKIP'
)

package() {
    install -Dm644 $srcdir/dns.conf $pkgdir/etc/NetworkManager/conf.d/dns.conf
    install -Dm644 $srcdir/80-ditana.conf $pkgdir/etc/ssh/sshd_config.d/80-ditana.conf
    install -Dm644 $srcdir/90-ditana.conf $pkgdir/etc/systemd/resolved.conf.d/90-ditana.conf
}
