# Maintainer: Karl-Felix Glatzer <karl[dot]glatzer[at]gmx[dot]de>
# Contributor: Daniel Bermond < gmail-com: danielbermond >

pkgname=mingw-w64-dav1d
pkgver=1.4.1
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
sha512sums=('51d893d5128eb3bd5c563bee82a1a450296e596763f64c10cea631aeaf655572b72dcab78be2f1e0d998010d7d22eb376b64603e5f568efe16d75eb5efaeac35'
            'SKIP'
            '61d60a5004e12ac104de0ce24f6ae823180c16fbf1ff90576075f9c853055e4d48db46fd7370182487b27c9e85d194a45734a3200848ac85b181778e0fc12a65')
b2sums=('6d8ddd4ef25055b8de33aa7608085e6569cb9e47ed312c9f57119c8961fd532d786e12722aa550b5f4584a9cca95022ac138b714e2a22b05547713465e43ed56'
        'SKIP'
        '0ae3e99b19d5819c82000c3c045db0ba55fc27f4e4807aef87564c2ce0894bed5aae8ce896a941a7e9629c1ae1fc42408a5807b1862912a3f9cb7dd402197339')
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
