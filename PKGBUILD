# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer:  Truocolo <truocolo@aol.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Dan McGee <dan@archlinux.org>
# Contributor: Daniele Paolella <dp@mcrservice.it>

_os="$( \
  uname -o)"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=virtualenv
_pkgname="${_pkg}"
pkgname="${_py}-${_pkg}"
pkgver=20.25.0
pkgrel=1
pkgdesc='Virtual Python Environment builder'
arch=(
  'any'
)
url="https://${_pkg}.pypa.io"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-distlib"
  "${_py}-filelock"
  "${_py}-platformdirs"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-filelock"
  "${_py}-hatchling"
  "${_py}-hatch-vcs"
  "${_py}-installer"
  "${_py}-sphinx"
  "${_py}-sphinx-argparse"
  "${_py}-sphinxcontrib-towncrier"
  "${_py}-wheel"
  'towncrier'
)
checkdepends=(
  'fish'
  "${_py}-coverage"
  "${_py}-flaky"
  "${_py}-pip"
  "${_py}-pytest"
  "${_py}-pytest-freezer"
  "${_py}-pytest-mock"
  "${_py}-time-machine"
  'tcsh'
  'xonsh'
)
replaces=(
  "${_pkg}"
)
conflicts=(
  "${_pkg}"
)
provides=(
  "${_pkg}"
)
options=(
  '!makeflags'
)
_commit='1941c1d5abf81814992b68bbc86c0020dc75a3ad'
_http="https://github.com"
_ns="pypa"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "${pkgname}::git+${_url}#commit=${_commit}"
)
b2sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${pkgname}"
  git \
    describe \
    --tags | \
    sed \
      's/^v//'
}

build() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
  # NOTE: install to tmp dir for documentation and tests
  "${_py}" \
    -m \
      installer \
    --destdir=test_dir \
    dist/*.whl
  PYTHONPATH="$(pwd)/test_dir/$site_packages:$PYTHONPATH" \
  sphinx-build \
    -b \
      man \
      docs \
      docs/_build/man
}

check() {
  local \
    site_packages \
    pytest_options=()
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  pytest_options=(
    -vv
    # tests try to find python2
    --deselect
      tests/unit/create/test_creator.py::test_py_pyc_missing[True-False]
    --deselect
      tests/unit/create/test_creator.py::test_py_pyc_missing[False-False]
    --deselect
      tests/unit/discovery/py_info/test_py_info.py::test_fallback_existent_system_executable
    --deselect
      tests/unit/test_util.py::test_reentrant_file_lock_is_thread_safe
  )
  cd \
    "${pkgname}"
  PYTHONPATH="$(pwd)/test_dir/$site_packages:$PYTHONPATH" \
  pytest \
    "${pytest_options[@]}"
}

package() {
  local \
    _ln_opts=() \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      dist/*.whl
  # man page
  install \
    -vDm644 \
    -t \
    "${pkgdir}/usr/share/man/man1" \
    docs/_build/man/virtualenv.1
  # sort out files with suffix of 3
  ln \
    -s \
    virtualenv.1.gz \
    "${pkgdir}/usr/share/man/man1/virtualenv3.1.gz"
  [[ "${_os}" != 'Linux' ]] && \
    _ln_opts=(
      -s
    )
  ln \
    "${_ln_opts[@]}" \
    "${pkgdir}/usr/bin/virtualenv" \
    "${pkgdir}/usr/bin/virtualenv3"
  # symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${pkgname#python-}-${pkgver}.dist-info/licenses/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set sw=2 sts=-1 et:
