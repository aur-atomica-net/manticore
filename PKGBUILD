# Maintainer: Jason R. McNeil <arch@jason.mcneil.dev>

pkgname='manticore'
pkgver=3.5.0
_release_archive='manticore-3.5.0-200722-1d34c49-release-source.tar.gz'
pkgrel=1
pkgdesc='High performance full-text search engine with SQL and JSON support'
arch=('x86_64')
url='https://manticoresearch.com/'
license=('GPL')
backup=('etc/conf.d/sphinx')
conflicts=('sphinx' 'sphinx-bin')
provides=('sphinx')
install='manticore.install'
makedepends=('cmake' 'mariadb-clients' 'postgresql-libs' 'git' 'python2' 'boost')
optdepends=('mariadb-clients: MySQL data source support'
  'postgresql-libs: PostgreSQL data source support')
source=(
    "https://repo.manticoresearch.com/repository/manticoresearch_source/release/${_release_archive}"
    'sphinx.conf.d'
    'manticore.install'
    'sphinx.service')
noextract=("${_release_archive}")
sha256sums=('00d65103d7f07d52b953731566c1015d5f668dd35e8709eeca10cc0fbedb9a66'
            'SKIP'
            'SKIP'
            'SKIP')

prepare() {
  mkdir -p "${srcdir}/${pkgname}"
  cd "${srcdir}/${pkgname}"
  tar --strip-components 1 -xf "${srcdir}/${_release_archive}"
}

build() {
  mkdir -p "${srcdir}/${pkgname}/build"
  cd "${srcdir}/${pkgname}/build"
  cmake -DUSE_GALERA=0 -DWITH_MYSQL=1 -DWITH_PGSQL=1 "${srcdir}/${pkgname}"
  make
}

package() {
  cd "${srcdir}/${pkgname}/build"

  # create directories
  install -d "$pkgdir"/usr/bin
  install -d "$pkgdir"/etc/sphinx
  install -d "$pkgdir"/usr/share/sphinx/lib
  install -d "$pkgdir"/var/lib/sphinx
  install -d "$pkgdir"/var/log/sphinx
  install -d "$pkgdir"/run/sphinx

  mv manticore.conf "${pkgdir}"/etc/sphinx/manticore.conf
  mv manticore-min.conf.dist "${pkgdir}"/etc/sphinx/manticore-min.conf

  mv api "${pkgdir}"/usr/share/sphinx/lib/api

  # create links
  install -Dm755 "src/index_converter" "${pkgdir}/usr/bin/index_converter"
  install -Dm755 "src/indexer" "${pkgdir}/usr/bin/indexer"
  install -Dm755 "src/indextool" "${pkgdir}/usr/bin/indextool"
  install -Dm755 "src/searchd" "${pkgdir}/usr/bin/searchd"
  install -Dm755 "src/spelldump" "${pkgdir}/usr/bin/spelldump"
  install -Dm755 "src/wordbreaker" "${pkgdir}/usr/bin/wordbreaker"

  # create service
  install -Dm644 "${srcdir}/sphinx.conf.d" "${pkgdir}/etc/conf.d/sphinx"
  install -Dm644 "${srcdir}/sphinx.service" "${pkgdir}/usr/lib/systemd/system/sphinx.service"
}
