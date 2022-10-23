# Maintainer: Dan McGee <dan@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Daniele Paolella <dp@mcrservice.it>

pkgname=python-virtualenv
pkgver=20.16.4
pkgrel=1
pkgdesc="Virtual Python Environment builder"
url="https://virtualenv.pypa.io/"
arch=('any')
license=('MIT')
depends=('python-distlib' 'python-filelock' 'python-platformdirs')
makedepends=('python-setuptools' 'python-setuptools-scm' 'python-sphinx' 'python-sphinx_rtd_theme'
             'python-sphinx-argparse' 'towncrier' 'python-importlib-metadata')
checkdepends=('python-pytest-freezegun' 'python-pytest-mock' 'python-pip' 'python-coverage' 'fish'
              'xonsh' 'python-flaky')  # 'tcsh' removed: randomly hangs tests
replaces=('virtualenv')
conflicts=('virtualenv')
options=('!makeflags')
source=(https://github.com/pypa/virtualenv/archive/$pkgver/$pkgname-$pkgver.tar.gz)
sha512sums=('6283618285687b01f6ccc3d0c0cf40f69fa815e6edab51c11503c0bda40d30ea4a608a1017d39c47a0155a4b582a83670bf396ec1dfa1e8e987c9aaeae895cc1')

export SETUPTOOLS_SCM_PRETEND_VERSION=$pkgver

prepare() {
  # workaround pip vendorod certifi
  sed -i "s|pkgutil.get_data(\"pip._vendor.certifi\", \"cacert.pem\")|open(os.path.join('/etc/ssl/certs/ca-certificates.crt'), 'rb').read()|" virtualenv-$pkgver/tests/conftest.py
}

build() {
  (cd virtualenv-$pkgver
    python setup.py build egg_info
    export PYTHONPATH="$PWD/build/lib:$PWD/src"
    sphinx-build -b man docs docs/_build/man
  )
}

check() {
  cd virtualenv-$pkgver
  # tests/unit/create/test_creator.py::test_py_pyc_missing: asking for Python 2.x
  PYTHONPATH="$PWD/build/lib:$PWD/src" python -m pytest --deselect tests/unit/create/test_creator.py::test_py_pyc_missing
}

package() {
  cd virtualenv-$pkgver
  python setup.py install --prefix=/usr --root="$pkgdir" --skip-build
  install -Dm 644 docs/_build/man/virtualenv.1 "${pkgdir}/usr/share/man/man1/virtualenv.1"
  ln -s virtualenv.1.gz "${pkgdir}/usr/share/man/man1/virtualenv3.1.gz"

  # link to a version with 3 suffix as well
  ln "$pkgdir/usr/bin/virtualenv" "$pkgdir/usr/bin/virtualenv3"

  install -D -m644 LICENSE -t \
    "$pkgdir"/usr/share/licenses/$pkgname/
}
