# $Id: PKGBUILD 235300 2015-03-31 16:50:11Z foutrelis $
# Maintainer: Jan de Groot <jgc@archlinux.org>

pkgbase=nss
pkgname=ca-certificates-mozilla-nolegacy
pkgver=3.19.1
pkgrel=0
pkgdesc="Mozilla's set of trusted CA certificates without legacy certificates re-enabled"
arch=(i686 x86_64)
url="http://www.mozilla.org/projects/security/pki/nss/"
license=('MPL' 'GPL')
_nsprver=4.10.7
provides=('ca-certificates-mozilla')
conflicts=('ca-certificates-mozilla')
depends=(ca-certificates-utils)
makedepends=('perl' 'python2')
options=('!strip' '!makeflags' 'staticlibs')
source=("https://ftp.mozilla.org/pub/security/nss/releases/NSS_${pkgver//./_}_RTM/src/nss-${pkgver}.tar.gz"
        certdata2pem.py
        bundle.sh)
sha256sums=('b7be709551ec13206d8e3e8c065b894fa981c11573115e9478fa051029c52fff'
            'af13c30801a8a27623948206458432a4cf98061b75ff6e5b5e03912f93c034ee'
            '045f520403f715a4cc7f3607b4e2c9bcc88fee5bce58d462fddaa2fdb0e4c180')

prepare() {
  mkdir certs

  cd nss-$pkgver

  ln -sr nss/lib/ckfw/builtins/certdata.txt ../certs/
  ln -sr nss/lib/ckfw/builtins/nssckbi.h ../certs/
}


build() {
  cd certs
  python2 ../certdata2pem.py

  cd ..
  sh bundle.sh
}

package() {
  install=ca-certificates-mozilla.install

  local _certdir="$pkgdir/usr/share/ca-certificates/trust-source"
  install -Dm644 ca-bundle.trust.crt "$_certdir/mozilla.trust.crt"
  install -Dm644 ca-bundle.neutral-trust.crt "$_certdir/mozilla.neutral-trust.crt"
  install -Dm644 ca-bundle.supplement.p11-kit "$_certdir/mozilla.supplement.p11-kit"
}
