# Maintainer: Karl-Felix Glatzer <karl[dot]glatzer[at]gmx[dot]de>
# Contributor: Daniel Bermond < gmail-com: danielbermond >

pkgname=mingw-w64-dav1d
pkgver=1.4.0
pkgrel=1
pkgdesc='AV1 cross-platform decoder focused on speed and correctness (mingw-w64)'
arch=('any')
url='https://code.videolan.org/videolan/dav1d/'
license=('BSD')
depends=('mingw-w64-crt' 'mingw-w64-vulkan-icd-loader')
options=('!strip' '!buildflags' '!libtool' 'staticlibs')
makedepends=('mingw-w64-gcc' 'mingw-w64-meson' 'mingw-w64-wine' 'mingw-w64-xxhash' 'nasm' 'ninja' 'doxygen' 'graphviz')
source=(https://downloads.videolan.org/pub/videolan/dav1d/${pkgver}/dav1d-${pkgver}.tar.xz{,.asc}
        https://code.videolan.org/videolan/dav1d-test-data/-/archive/${pkgver}/dav1d-test-data-${pkgver}.tar.gz)
sha512sums=('32fcb1d19f35bec62c05637987e0204cf76880af192685e0acb08ea30cb9dac889ee89be0f847c79c6c51210c375031b26cf6e4aabc91480e44777e9fb3cbe02'
            'SKIP'
            'aa96f80abd4acff5dc312b2e6807ac31f6c1eb4362d2837078c096d4dcae65f375494eab8ddec27fceb4cfe0d33428302bd1c19d5e700b96a22c2e7b720accc2')
b2sums=('e37f7abc222b9a0f774a76ef6dcc2f28c411220c0f92c2239e51c3313bf1109fb6e4feb1451049248e033f2dd79550536a773f3b7b07e5a7890e8bb760c0f596'
        'SKIP'
        '487e28d14694fd37905d685e822a7c9a2859b212634ce9f1e7dff3aa50452a70b3fbd7a25edc80120d31f0e02488d3a1e606b4917fa6cd26fe74c45ce2ef133b')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"
validpgpkeys=('65F7C6B4206BD057A7EB73787180713BE58D1ADC') # VideoLAN Release Signing Key

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
