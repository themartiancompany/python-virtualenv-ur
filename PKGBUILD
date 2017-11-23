# Maintainer: Dan McGee <dan@archlinux.org>
# Contributor: Daniele Paolella <dp@mcrservice.it>

pkgname=('python-virtualenv' 'python2-virtualenv')
pkgver=15.1.0
pkgrel=2
pkgdesc="Virtual Python Environment builder"
url="https://virtualenv.pypa.io/"
arch=('any')
license=('MIT')
makedepends=('python' 'python-sphinx' 'python2' 'python2-sphinx')
checkdepends=('python-pytest' 'python-mock' 'python2-pytest' 'python2-mock')
replaces=('virtualenv')
conflicts=('virtualenv')
options=('!makeflags')
source=(${pkgname}-${pkgver}.tar.gz::https://github.com/pypa/virtualenv/archive/${pkgver}.tar.gz)
md5sums=('30222e271963a437e240aee4853728d2')
sha256sums=('aea627d114a3863d6374c5a3fc3cdd08907e0ac951cf93b458e5ba5998c516de')

prepare() {
  cp -a virtualenv-${pkgver}{,-py2}
  cd virtualenv-${pkgver}-py2
  sed -i "s|#!/usr/bin/env python$|#!/usr/bin/env python2|" virtualenv.py
}

build() {
  (cd virtualenv-${pkgver}
    python setup.py build
    make -C docs text man
  )
  (cd virtualenv-${pkgver}-py2
    python2 setup.py build
    make -C docs text man
  )
}

check() {
  (cd virtualenv-${pkgver}
    py.test
  )
  (cd virtualenv-${pkgver}-py2
    py.test2
  )
}

package_python-virtualenv() {
  depends=('python')

  cd virtualenv-$pkgver
  python setup.py install --prefix=/usr --root="$pkgdir" --skip-build
  install -Dm 644 docs/_build/text/* -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -Dm 644 docs/_build/man/virtualenv.1 "${pkgdir}/usr/share/man/man1/virtualenv.1"
  ln -s virtualenv.1.gz "${pkgdir}/usr/share/man/man1/virtualenv3.1.gz"

  # link to a version with 3 suffix as well
  ln "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv3"

  install -D -m644 LICENSE.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_python2-virtualenv() {
  depends=('python2')

  cd virtualenv-$pkgver-py2
  python2 setup.py install --prefix=/usr --root="$pkgdir" --skip-build
  install -Dm 644 docs/_build/text/* -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -Dm 644 docs/_build/man/virtualenv.1 "${pkgdir}/usr/share/man/man1/virtualenv2.1"

  # move this "old" version out of the way
  mv "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv2"

  install -D -m644 LICENSE.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
