# Maintainer: Karl-Felix Glatzer <karl[dot]glatzer[at]gmx[dot]de>
# Contributor: Daniel Bermond < gmail-com: danielbermond >

pkgname=mingw-w64-dav1d
pkgver=1.4.2
pkgrel=1
pkgdesc='AV1 cross-platform decoder focused on speed and correctness (mingw-w64)'
arch=('any')
url='https://code.videolan.org/videolan/dav1d/'
license=('BSD-2-Clause')
depends=('mingw-w64-crt' 'mingw-w64-vulkan-icd-loader')
options=('!strip' '!buildflags' '!libtool' 'staticlibs')
makedepends=(
  'mingw-w64-gcc'
  'mingw-w64-meson'
  'mingw-w64-wine'
  'mingw-w64-xxhash'
  'nasm'
  'ninja'
  'doxygen'
  'graphviz'
)
source=(
  https://downloads.videolan.org/pub/videolan/dav1d/${pkgver}/dav1d-${pkgver}.tar.xz{,.asc}
  https://code.videolan.org/videolan/dav1d-test-data/-/archive/${pkgver}/dav1d-test-data-${pkgver}.tar.gz
)
sha512sums=('36f8e632f64212db63e126b9ed2ca95c2db930d2388c551cde0b13b783ae872edeab1b5dd2274684bb03e98536d0334552ddcba209cff08c3568d97321034920'
            'SKIP'
            '8e3060106de265eb38441e3209b1d7e2b934afd212e9adeffc73e774df4c6e03f28582d66ae665ad7256fe61074fb9778897a137df7af5acb9cfc89291b308ae')
b2sums=('03c8effb21df5c748f6e836413454577ac0920dce37d6e4922c2a0e493f79c9bb9fb6eabe05e2ac0f0f7089751bec1eb92717305c5faac1b71efd7b0dabe6be7'
        'SKIP'
        '8b1f09b5dd16f76788e5247fb69402901ad6ed57788e9b36113b05fa7dace1d35b20cf2a895aa2ec573cc653563a149aca9cc8f32f448e2ff57a3b5a60d36700')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"
validpgpkeys=(
  '65F7C6B4206BD057A7EB73787180713BE58D1ADC' # VideoLAN Release Signing Key
)

prepare() {
  cd "${srcdir}/dav1d-${pkgver}"
  ln -sf "${srcdir}/dav1d-test-data-${pkgver}" tests/dav1d-test-data
}

build() {
  export NEED_WINE=1
  export WINEDEBUG=-all
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}/dav1d-${pkgver}/build-${_arch}" && cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
    ${_arch}-meson \
      --default-library both \
      -Dtestdata_tests=true \
      -Denable_docs=false \
      ..

    ninja
  done
}

# check disabled since a few tests run into timeout and one test fails (both probably wine related)
# majority of tests does pass though
#check() {
#  for _arch in ${_architectures}; do
#    cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
#
#    export WINEDEBUG=-all
#    ln -sf "${srcdir}/dav1d-${pkgver}/build-${_arch}/src/libdav1d.dll" ./libdav1d.dll
#    meson test
#  done
#}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
    
    DESTDIR="$pkgdir" ninja install
  done

  cd "${srcdir}/dav1d-${pkgver}"
  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -D -m644 doc/PATENTS -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
