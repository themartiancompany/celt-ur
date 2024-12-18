# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

pkg=celt
pkgname="${_pkg}"
pkgver=0.11.3
pkgrel=7
pkgdesc='Low-latency audio communication codec'
arch=(
  'x86_64'
  'i686'
  'aarch64'
  'arm'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
)
_proj="xiph"
_http="https://gitlab.${_proj}.org"
_ns="${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD-2-Clause'
)
depends=(
  'libogg'
)
source=(
  "${url}/-/archive/v${pkgver}/${_pkg}-v${pkgver}.tar.bz2"
  "010-${_pkg}-fix-tandem-test.patch"
)
sha256sums=(
  'e11a3ff390a733a470ca3ceb1f74c19770ee7c6f0ee169fdebc7b6efdc8700be'
  '233418e1cff4673e374a67ef7ee201a27f40ae38412a006ccd2d21882f512992'
)

prepare() {
  patch \
    -d \
      "${_pkg}-v${pkgver}" \
    -Np1 \
    -i \
    "${srcdir}/010-${_pkg}-fix-tandem-test.patch"
  "${pkgname}-v${pkgver}/autogen.sh"
}

build() {
  # fix for tandem-test
  export \
    CFLAGS="${CFLAGS/-Wp,-D_FORTIFY_SOURCE=?/}"
  cd \
    "${_pkg}-v${pkgver}"
  ./configure \
    --prefix='/usr' \
    --enable-custom-modes
  make
}

check() {
  # fix for tandem-test
  export \
    CFLAGS="${CFLAGS/-Wp,-D_FORTIFY_SOURCE=?/}"
  make \
    -C \
      "${_pkg}-v${pkgver}" \
    check
}

package() {
  make \
    -C "${_pkg}-v${pkgver}" \
    DESTDIR="${pkgdir}" \
    install
  install \
    -Dm644 \
    "${_pkg}-v${pkgver}/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
