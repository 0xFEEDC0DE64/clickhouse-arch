# $Id: $
# Maintainer: Dmitry Bilunov <kmeaw@yandex-team.ru>

pkgname=clickhouse
pkgver=18.10.3
pkgrel=2
pkgdesc='An open-source column-oriented database management system that allows generating analytical data reports in real time'
arch=('i686' 'x86_64')
url='https://clickhouse.yandex/'
license=('Apache')
depends=('ncurses' 'readline' 'unixodbc' 'termcap' 'double-conversion' 'capnproto' 're2' 'gtest')
makedepends=('cmake')
source=(https://github.com/yandex/ClickHouse/archive/v$pkgver-stable.tar.gz
        https://github.com/google/cctz/archive/4f9776a.tar.gz
        https://github.com/edenhill/librdkafka/archive/7478b5e.tar.gz
        https://github.com/lz4/lz4/archive/c10863b.tar.gz
        https://github.com/facebook/zstd/archive/2555975.tar.gz
        https://github.com/Dead2/zlib-ng/archive/9173b89.tar.gz
        https://github.com/ClickHouse-Extras/poco/archive/3df9473.tar.gz
        https://github.com/ClickHouse-Extras/boost/archive/2d5cb2c.tar.gz
        https://github.com/ClickHouse-Extras/ssl/archive/de02224.tar.gz
        https://github.com/jemalloc/jemalloc/archive/41b7372.tar.gz
        libunwind.patch)
md5sums=('07fc1025d2b6b6b5863d8b9521e41c91'
         '5323f7ba2565a84a80a93edde95eb4fe'
         '7d9c187a8afedde260fadf2f1d5f46a2'
         '7b92f0554687e6a8949adc5c10aeff78'
         'aaa86ec9f379ef587cc53f7b96bcc0e7'
         '8a7abcc6998e461605ecb2988ff93dfc'
         '35c616621ff836091fa212bedb4fb758'
         '123c1e981fe28c1240d066af493d83b9'
         '26744168b84de285c9e7c0f3d3ae5ba0'
         '6eaa07f5751d6264208185224f7a5e51'
         'f3f60b75abf8d6f21de74db6e88e1e7b')
backup=('etc/clickhouse-client/config.xml' 'etc/clickhouse-server/config.xml' 'etc/clickhouse-server/users.xml')
install=$pkgname.install

prepare() {
  cd ClickHouse-$pkgver-stable
  sed -e 's/mysqlxx common\(.*\) \(\${Z_LIB}\)/mysqlxx \2 common\1/' -i libs/libmysqlxx/CMakeLists.txt
  patch -p1 < ../libunwind.patch
  mkdir -p contrib/cctz contrib/librdkafka contrib/lz4 contrib/zstd
  rm -rf contrib/{cctz,librdkafka,lz4,zstd,zlib-ng,poco,boost}/*
  mv ../cctz-4f9776a*/* contrib/cctz/
  mv ../librdkafka-7478b5e*/* contrib/librdkafka/
  mv ../lz4-c10863b*/* contrib/lz4/
  mv ../zstd-2555975*/* contrib/zstd/
  mv ../zlib-ng-9173b89*/* contrib/zlib-ng/
  mv ../poco-3df9473*/* contrib/poco/
  mv ../boost-2d5cb2c*/* contrib/boost/
  mv ../ssl-de02224*/* contrib/ssl/
  mv ../jemalloc-41b7372*/* contrib/jemalloc/
  for dir in contrib/*/; do
    rmdir $dir &> /dev/null || true
  done
}

build() {
  cd ClickHouse-$pkgver-stable
  cmake -D CMAKE_BUILD_TYPE:STRING=Release -D USE_STATIC_LIBRARIES:BOOL=False -D ENABLE_TESTS:BOOL=False -D UNBUNDLED:BOOL=False -D USE_INTERNAL_DOUBLE_CONVERSION_LIBRARY:BOOL=False -D USE_INTERNAL_CAPNP_LIBRARY:BOOL=False -D USE_INTERNAL_POCO_LIBRARY:BOOL=True -D POCO_STATIC:BOOL=True -D USE_INTERNAL_RE2_LIBRARY:BOOL=False -D NO_WERROR=1 .
  make clickhouse
}

package() {
  cd ClickHouse-$pkgver-stable
  mkdir -p $pkgdir/etc/clickhouse-server/ $pkgdir/etc/clickhouse-client/
  mkdir -p $pkgdir/usr/bin/
  mkdir -p $pkgdir/usr/lib/systemd/system
  ln -s clickhouse-client $pkgdir/usr/bin/clickhouse-server
  cp dbms/programs/server/config.xml dbms/programs/server/users.xml $pkgdir/etc/clickhouse-server/
  cp dbms/programs/clickhouse $pkgdir/usr/bin/clickhouse-client
  cp dbms/programs/client/clickhouse-client.xml $pkgdir/etc/clickhouse-client/config.xml
  cp dbms/libclickhouse.so.18 $pkgdir/usr/lib/libclickhouse.so.$pkgver
  cp contrib/zlib-ng/libz-ng.so.1 $pkgdir/usr/lib/libz-ng.so.1
  sed -e 's:/opt/clickhouse:/var/lib/clickhouse:g' -i $pkgdir/etc/clickhouse-server/config.xml
  sed -e '/listen_host/s%::<%::1<%' -i $pkgdir/etc/clickhouse-server/config.xml
  cp debian/clickhouse-server.service $pkgdir/usr/lib/systemd/system
}

# vim:set ts=2 sw=2 et:
