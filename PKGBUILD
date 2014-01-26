# Maintainer: Dan McGee <dan@archlinux.org>
# Contributor: Daniele Paolella <dp@mcrservice.it>

pkgname=('python-virtualenv' 'python2-virtualenv')
pkgver=1.11.2
pkgrel=1
pkgdesc="Virtual Python Environment builder"
url="http://www.virtualenv.org/"
arch=('any')
license=('MIT')
makedepends=('python' 'python2')
replaces=('virtualenv')
conflicts=('virtualenv')
source=("http://pypi.python.org/packages/source/v/virtualenv/virtualenv-$pkgver.tar.gz")

package_python-virtualenv() {
  depends=('python')

  cd "$srcdir/virtualenv-$pkgver"
  LANG='en_US.UTF-8' python3 setup.py build
  LANG='en_US.UTF-8' python3 setup.py install --prefix=/usr --root="$pkgdir"

  # link to a version with 3 suffix as well
  ln "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv3"

  install -D -m644 LICENSE.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_python2-virtualenv() {
  depends=('python2')

  cd "$srcdir/virtualenv-$pkgver"
  python2 setup.py build
  python2 setup.py install --prefix=/usr --root="$pkgdir"

  # move this "old" version out of the way
  mv "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv2"

  # should report this upstream as still not fixed...
  sed -i "s|#!/usr/bin/env python$|#!/usr/bin/env python2|" \
    $pkgdir/usr/lib/python2.7/site-packages/virtualenv.py
 
  install -D -m644 LICENSE.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

md5sums=('d3d915836c1ada1be731ccaa12412b98')
sha256sums=('3080a1610fa8c4ab23008707ca5c96d7e67d8cb3ed98c7e01e97f542c82bb6df')
