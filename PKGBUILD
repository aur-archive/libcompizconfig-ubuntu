# Maintainer: Xiao-Long Chen <chenxiaolong@cxl.epac.to>

pkgname=libcompizconfig-ubuntu

_ubuntu_rel=0ubuntu6
_ubuntu_ver='~bzr428'
_actual_ver=0.9.7.0
_WTF_ver=0.9.5.94

pkgver=${_actual_ver}.bzr428.${_ubuntu_rel}

pkgrel=100
pkgdesc="Compiz configuration system library - Ubuntu version"
arch=('i686' 'x86_64')
url="http://compiz.org"
license=('GPL')
depends=('compiz-core-ubuntu' 'protobuf')
makedepends=('intltool' 'pkgconfig' 'cmake')
provides=('libcompizconfig')
conflicts=('libcompizconfig')
groups=('unity')
source=("https://launchpad.net/ubuntu/+archive/primary/+files/${pkgname%-*}_${_actual_ver}${_ubuntu_ver}.orig.tar.bz2"
        "https://launchpad.net/ubuntu/+archive/primary/+files/${pkgname%-*}_${_actual_ver}${_ubuntu_ver}-${_ubuntu_rel}.debian.tar.gz")
sha512sums=('13612ebcdd3016c5111a664487dd76412ef39f59fe986c0805a96d58696780d200e70dfd09e2d93a20e4c35aeb5cd2959a79c6b544d465c9232a60889b692e19'
            '2a92b6551466b5be6b0e99a11c1d985b569d14d898ecb78e9ce953de2e82129e2f16aa2446dbd86339a64e0ff4403aa218bda0acce65cb6767b1bd052f518bb8')

build() {
  cd "${srcdir}/${pkgname%-*}-${_WTF_ver}"

  # Apply Ubuntu patches
  for i in $(cat "${srcdir}/debian/patches/series" | grep -v '#'); do
    patch -Np1 -i "${srcdir}/debian/patches/${i}"
  done

  CXXFLAGS="${CXXFLAGS} $(pkg-config --cflags --libs compiz)"

  [[ -d build ]] || mkdir build
  cd build
  cmake .. \
    -DCMAKE_BUILD_WITH_RPATH=FALSE \
    -DCOMPIZ_PACKAGING_ENABLED=TRUE \
    -DCOMPIZ_PLUGIN_INSTALL_TYPE=package \
    -DUSE_GSETTINGS=OFF \
    -DCOMPIZ_DISABLE_GS_SCHEMAS_INSTALL=ON \
    -DCMAKE_BUILD_TYPE="Release" \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCOMPIZ_DESTDIR="${pkgdir}"

  make ${MAKEFLAGS}
}

package() {
  cd "${srcdir}/${pkgname%-*}-${_WTF_ver}"
  cd "build"
  make install
  make findcompizconfig_install

  install -dm755 "${pkgdir}/usr/share/compizconfig"
  install -dm755 "${pkgdir}/etc/compizconfig"
  install -m644 "${srcdir}/debian/normal.profile" \
    "${pkgdir}/usr/share/compizconfig/"
  install -m644 "${srcdir}/debian/extra.profile" \
    "${pkgdir}/usr/share/compizconfig/"
  install -m644 "../config/config" "${pkgdir}/etc/compizconfig/"
}

# vim:set ts=2 sw=2 et:
