# Maintainer: Dmitry Bilunov <kmeaw@yandex-team.ru>
# Maintainer: Mikhail f. Shiryaev <mr dot felixoid at gmail dot com>

pkgname=clickhouse
pkgver=22.2.3.5
pkgrel=1
pkgdesc='An open-source column-oriented database management system that allows generating analytical data reports in real time'
arch=('x86_64')
url='https://clickhouse.tech/'
license=('Apache')
depends=('tzdata' 'libcap')
noextract=(
  clickhouse-client_${pkgver}_all.deb
  clickhouse-common-static_${pkgver}_amd64.deb
  clickhouse-server_${pkgver}_all.deb
)
source=(
  https://github.com/ClickHouse/ClickHouse/releases/download/v${pkgver}-stable/clickhouse-client_${pkgver}_all.deb
  https://github.com/ClickHouse/ClickHouse/releases/download/v${pkgver}-stable/clickhouse-common-static_${pkgver}_amd64.deb
  https://github.com/ClickHouse/ClickHouse/releases/download/v${pkgver}-stable/clickhouse-server_${pkgver}_all.deb
)
sha256sums=(
  af55926455988436cbb21899c683cb1326cad3e4901e28b5f5b71244a2a9c58c
  287411ceff16bbd61294864b3b3a17b3f233a67fb97ac388191e7a81629d2e92
  0c0544bdeaa8b287d18dcb5c17883ab3a5de58a25137ce8541bf95c0945fdccf
)
install=$pkgname.install
backup=(
  'etc/clickhouse-client/config.xml'
  'etc/clickhouse-server/config.xml'
  'etc/clickhouse-server/users.xml'
)

package() {
  for deb in "${noextract[@]}"; do
    bsdtar -xf $deb
    tar xf data.tar.gz -C "${pkgdir}"
  done

  gzip -d "${pkgdir}/usr/share/doc/${pkgname}-server/LICENSE.gz"
  mkdir -p "${pkgdir}/usr/share/licenses/${pkgname}"
  mv "${pkgdir}/lib" "${pkgdir}/usr/lib"
  mv "${pkgdir}/usr/share/doc/clickhouse-server/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/"

  rm -rf "${pkgdir}/etc/cron.d" \
    "${pkgdir}/etc/init.d" \
    "${pkgdir}/etc/security" \
    "${pkgdir}/etc/systemd" \
    "${pkgdir}/usr/share/doc"
}

# vim:set ts=2 sw=2 et:
