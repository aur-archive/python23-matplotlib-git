# Maintainer: Daan <d.r.vanrossum at gmx dot de>
# Contributor: Mitchel Humpherys <mitch.special@gmail.com>
# Contributor: Hector Martinez-Seara Monne <hseara##a##gmail##.##com>

# optional dependencies during build:
#  'python2-dateutil: extensions to the datetime module'
#  'python2-pytz: timezone library'
#  'python2-pyparsing'
#  'python2-qt: for use with the QT backend'
#  'pygtk: for use with the GTK or GTKAgg backend'
#  'tk: used by the TkAgg backend'
#  'wxpython: for use with the WXAgg backend'
#  'ghostscript: for creation of EPS files'
#  'latex: for latex in figures'

pkgbase=python-matplotlib-git
pkgname=('python23-matplotlib-git')
true && pkgname=('python2-matplotlib-git' 'python-matplotlib-git')
pkgver=1.4
pkgrel=1
pkgdesc="A python plotting library (dual python2/python3)"
url="https://github.com/matplotlib/matplotlib"
arch=('i686' 'x86_64')
license=('custom')
makedepends=('git' 'freetype2' 'libpng' 'python-numpy' 'python2-numpy')
source=('setup.cfg')
md5sums=('54c062ae3c6de2b506e2e5fdd224d60e')

_gitroot='https://github.com/matplotlib/matplotlib.git'
_gitname='matplotlib'

build() {
  cd "$srcdir"
  msg "Connecting to git server..."
  if [[ -d $_gitname ]]; then
        cd $_gitname && git pull origin
        msg "The local files are up-to-date"
  else
        git clone $_gitroot $_gitname --depth=1
        cd $_gitname
  fi

  echo "Building Python2"
  rm -rf "$srcdir/$_gitname-py2-build"
  cp -pR "$srcdir/$_gitname" "$srcdir/$_gitname-py2-build"
  cd "$srcdir/$_gitname-py2-build"
  cp ../setup.cfg .
  # python2 fix
  for file in $(find . -name '*.py' -print); do
    sed -i -e "s|^#!.*/usr/bin/python|#!/usr/bin/python2|" \
           -e "s|^#!.*/usr/bin/env *python|#!/usr/bin/env python2|" ${file}
  done
  python2 setup.py build

  echo "Building Python3"
  rm -rf "$srcdir/$_gitname-build"
  cp -pR "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"
  cp ../setup.cfg .
  python setup.py build
}


package_python2-matplotlib-git() {
  depends=('python2-numpy' 'freetype2' 'libpng')
  replaces=('python2-matplotlib')
  conflicts=('python2-matplotlib')
  provides=('python2-matplotlib')

  cd "$srcdir/$_gitname-py2-build"
  python2 setup.py install -O1 --skip-build --root "${pkgdir}" --prefix=/usr
   
  install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m 644 doc/users/license.rst "${pkgdir}/usr/share/licenses/${pkgname}"
}

package_python-matplotlib-git() {
  depends=('python-numpy' 'freetype2' 'libpng')
  #provides=('python-matplotlib') #conflicts with python2-matplotlib
  provides=('python3-matplotlib')

  cd "$srcdir/$_gitname-build"
  python setup.py install -O1 --skip-build --root "${pkgdir}" --prefix=/usr

  install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m 644 doc/users/license.rst "${pkgdir}/usr/share/licenses/${pkgname}"
}
