# Maintainer: Jason R. McNeil <jason@jasonrm.net>

pkgname='manticore'
pkgver=2.8.1
_release_archive='manticore-2.8.1-190306-3684198-release.tar.gz'
pkgrel=1
pkgdesc='High performance full-text search engine with SQL and JSON support'
arch=('x86_64')
url='https://manticoresearch.com/'
license=('GPL')
backup=('etc/conf.d/sphinx')
conflicts=('sphinx' 'sphinx-bin')
provides=('sphinx')
install='manticore.install'
makedepends=('cmake' 'mariadb-clients' 'postgresql-libs' 'git' 'python2')
optdepends=('mariadb-clients: MySQL data source support'
  'postgresql-libs: PostgreSQL data source support')
source=(
    "https://github.com/manticoresoftware/manticoresearch/releases/download/${pkgver}/${_release_archive}"
    'sphinx.conf.d'
    'manticore.install'
    'sphinx.service')
sha256sums=('d0ae4bc106ec223a6c30eec6255e54e93824e2191215a20fd6a5da40359fe3b1'
            'SKIP'
            'SKIP'
            'SKIP')

build() {
  cd "${srcdir}"

  mkdir -p "${srcdir}/build"
  cd build

  cmake -D WITH_MYSQL=1 WITH_PGSQL=1 "${srcdir}/${_release_archive%.tar.gz}"
  make
}

package() {
  cd "${srcdir}/${_release_archive%.tar.gz}"

  # create directories
  install -d "$pkgdir"/usr/bin
  install -d "$pkgdir"/etc/sphinx
  install -d "$pkgdir"/usr/share/sphinx/lib
  install -d "$pkgdir"/var/lib/sphinx
  install -d "$pkgdir"/var/log/sphinx
  install -d "$pkgdir"/run/sphinx

  mv sphinx.conf.in "${pkgdir}"/etc/sphinx/sphinx.conf
  mv sphinx-min.conf.in "${pkgdir}"/etc/sphinx/sphinx-min.conf

  mv api "${pkgdir}"/usr/share/sphinx/lib/api
  mv doc "${pkgdir}"/usr/share/sphinx/lib/doc

  cd "${srcdir}/build/src/"

  # create links
  install -Dm755 "indexer" "${pkgdir}/usr/bin/indexer"
  install -Dm755 "indextool" "${pkgdir}/usr/bin/indextool"
  install -Dm755 "searchd" "${pkgdir}/usr/bin/searchd"
  install -Dm755 "spelldump" "${pkgdir}/usr/bin/spelldump"
  install -Dm755 "wordbreaker" "${pkgdir}/usr/bin/wordbreaker"

  # create service
  install -Dm644 "${srcdir}/sphinx.conf.d" "${pkgdir}/etc/conf.d/sphinx"
  install -Dm644 "${srcdir}/sphinx.service" "${pkgdir}/usr/lib/systemd/system/sphinx.service"
}
