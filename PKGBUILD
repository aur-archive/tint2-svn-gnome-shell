# Contributor: janhouse <boss@janhouse.lv>
# Contributor: zoulnix <http://goo.gl/HQaP>
# Original maintainer: vicky91 <vickypaiers@gmail.com>
pkgname=tint2-svn-gnome-shell
pkgver=646
pkgrel=1
pkgdesc="A basic, good-looking task manager for WMs. Patched for gnome shell (in multitaskbar mode, in vertical panel, makes last taskbar smaller to save screen space)."
arch=('i686' 'x86_64')
url="http://code.google.com/p/tint2/"
license=('GPL')
depends=('libxcomposite' 'libxdamage' 'libxinerama' 'libxrandr' 'pango' 'imlib2')
makedepends=('cmake' 'subversion')
options=('!libtool')
provides=('tint2')
conflicts=('tint2' 'tint' 'ttm-svn' 'tint2-svn')
source=()
md5sums=('')

_svnmod="tint2"
_svntrunk="http://tint2.googlecode.com/svn/trunk"

build() {
  cd ${srcdir}

  #####
  msg "Getting sources..."
  if [ -d ${_svnmod}/.svn ]; then
    cd ${_svnmod} && svn up -r ${pkgver}
  else
    svn co ${_svntrunk} --config-dir ./ -r ${pkgver} ${_svnmod}
    cd ${_svnmod}
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."
  #####
    patch -p1 -i ../../tint2.patch
    patch -p1 -i ../../tint2-gnome3.patch

  # python2 fix
  sed -i 's_#!/usr/bin/env python_#!/usr/bin/env python2_' src/tint2conf/tintwizard.py
  sed -i 's_python _python2 _' src/tint2conf/main.c

  cmake . -DCMAKE_INSTALL_PREFIX=/usr \
	  -DENABLE_TINT2CONF=1 || return 1

  make || return 1
}

package() {
  cd ${srcdir}/${_svnmod}

  make DESTDIR=${pkgdir} install || return 1
}
