# Maintainer: Dan McGee <dan@archlinux.org>
# Contributor: Daniele Paolella <dp@mcrservice.it>

pkgname=('python-virtualenv' 'python2-virtualenv')
pkgver=1.7.2
pkgrel=1
pkgdesc="Virtual Python Environment builder"
url="http://www.virtualenv.org/"
arch=('any')
license=('MIT')
makedepends=('python' 'python-distribute' 'python2' 'python2-distribute')
replaces=('virtualenv')
conflicts=('virtualenv')
source=("http://pypi.python.org/packages/source/v/virtualenv/virtualenv-$pkgver.tar.gz")

package_python-virtualenv() {
  depends=('python' 'python-distribute')
  install=python-virtualenv.install

  cd "$srcdir/virtualenv-$pkgver"
  LANG='en_US.UTF-8' python setup.py build
  LANG='en_US.UTF-8' python setup.py install --prefix=/usr --root="$pkgdir"

  # link to a version with 3 suffix as well
  ln "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv3"

  install -D -m644 LICENSE.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

package_python2-virtualenv() {
  depends=('python2' 'python2-distribute')

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

md5sums=('b5d63b05373a4344ae099a68875aae78')
sha256sums=('8cbbe308ab0f446a4ceacf24b2656bedd33ea22825b0750fd079766cc6e91bf4')
