# Maintainer: Karl-Felix Glatzer <karl[dot]glatzer[at]gmx[dot]de>
# Contributor: Daniel Bermond < gmail-com: danielbermond >

pkgname=mingw-w64-dav1d
pkgver=0.5.1
pkgrel=1
pkgdesc='AV1 cross-platform decoder focused on speed and correctness (mingw-w64)'
arch=('i686' 'x86_64')
url='https://code.videolan.org/videolan/dav1d/'
license=('BSD')
depends=('mingw-w64-crt' 'mingw-w64-vulkan-icd-loader')
options=('!strip' '!buildflags' '!libtool' 'staticlibs')
makedepends=('mingw-w64-gcc' 'mingw-w64-meson' 'mingw-w64-wine' 'git' 'nasm' 'doxygen')
source=(https://downloads.videolan.org/pub/videolan/dav1d/${pkgver}/dav1d-${pkgver}.tar.xz{,.asc}
        mingw-w64-stripping-fix.patch)
sha512sums=('0ed67219127c2a3dc3151d9e427c5f3dab4e3272a8fdcd17ceae032b81992a1f250045d45b0e0c5c500ebf11e8387fd47f8bf8c4c319577c887daab49a651504'
            'SKIP'
            '04dba9898475550243b6edd7dade239f67c132f0bc5c0f2ac57b60de1c9d22b5c3e4cec16aa00ad505b47d36142278bf36a1efc6c7f424c039cf1a7c87cb3cbf')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"
validpgpkeys=('65F7C6B4206BD057A7EB73787180713BE58D1ADC') # VideoLAN Release Signing Key

prepare() {
  cd "${srcdir}/dav1d-${pkgver}"

  patch -Np1 -i "${srcdir}/mingw-w64-stripping-fix.patch"
}

build () {
  export NEED_WINE=1
  export WINEDEBUG=-all
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}/dav1d-${pkgver}/build-${_arch}" && cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
    ${_arch}-meson .. --default-library both
    
    ninja
  done
}

check() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
    
    export WINEDEBUG=-all
    meson test
  done
}

package () {
  for _arch in ${_architectures}; do
    cd "${srcdir}/dav1d-${pkgver}/build-${_arch}"
    
    DESTDIR="$pkgdir" ninja install
  done

  cd "${srcdir}/dav1d-${pkgver}"
  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -D -m644 doc/PATENTS -t "${pkgdir}/usr/share/licenses/${pkgname}"
}