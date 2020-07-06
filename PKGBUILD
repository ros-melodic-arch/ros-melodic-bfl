# Maintainer: Firas Zaidan <firas@zaidan.de>
pkgdesc="ROS - This package contains a recent version of the Bayesian Filtering Library (BFL), distributed by the Orocos Project."
url='https://wiki.ros.org/bfl'

pkgname='ros-melodic-bfl'
pkgver='0.8.0'
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'armv6h')
pkgrel=4
license=('LGPL')

ros_makedepends=()
makedepends=('cmake' 'git' 'ros-build-tools'
  ${ros_makedepends[@]}
  boost
  cppunit)

ros_depends=(ros-melodic-catkin)
depends=(${ros_depends[@]}
  boost
  cppunit)

# Git version (e.g. for debugging)
# _tag=release/melodic/bfl/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/ros-gbp/bfl-release.git"#tag=${_tag})
# sha256sums=('SKIP')

# Tarball version (faster download)
_dir="bfl-release-upstream-${pkgver}"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ros-gbp/bfl-release/archive/upstream/${pkgver}.tar.gz")
sha256sums=('9720725d0a2b5f28a3c65ced1abf4623532b7af2d33934d05146fd6c0fce8b04')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
