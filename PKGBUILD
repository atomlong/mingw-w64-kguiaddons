pkgname=mingw-w64-kguiaddons
pkgver=5.23.0
pkgrel=1
arch=(any)
pkgdesc="Addons to QtGui (mingw-w64)"
license=("LGPL")
depends=(mingw-w64-qt5-base)
groups=(mingw-w64-kf5)
makedepends=(mingw-w64-cmake mingw-w64-extra-cmake-modules)
options=(staticlibs !strip !buildflags)
url="https://projects.kde.org/projects/frameworks/kguiaddons"
source=("http://download.kde.org/stable/frameworks/${pkgver%.*}/kguiaddons-${pkgver}.tar.xz")
md5sums=('e910e142696eec33aec7f57fce6efba5')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
	cd kguiaddons-$pkgver
	for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir "build-${_arch}" && pushd "build-${_arch}"
    ${_arch}-cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
      -DBUILD_TESTING=OFF \
      ..
    make
    popd
  done
}

package() {
	for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname#mingw-w64-}-$pkgver/build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.dll' -exec ${_arch}-strip --strip-unneeded {} \;
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs ${_arch}-strip -g
    rm -rf "$pkgdir/usr/${_arch}/share"
  done
}
