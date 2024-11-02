# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

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
_pkg=sphinxcontrib-htmlhelp
pkgname="${_py}-${_pkg}"
pkgver=2.1.0
pkgrel=1
pkgdesc='Sphinx extension which renders HTML help files'
arch=(
  any
)
_http="https://github.com"
_ns="sphinx-doc"
url="${_http}/${_ns}/${_pkg}"
license=(
  BSD-2-Clause
)
license=(BSD-2-Clause)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  git
  "${_py}-build"
  "${_py}-flit-core"
  "${_py}-installer"
)
checkdepends=(
  "${_py}-html5lib"
  "${_py}-pytest"
  "${_py}-sphinx"
)
provides=(
  "${_py}-${_pkgname}=${pkgver}"
)
source=(
  "${_pkg}-${pkgver}::git+${url}.git#tag=$pkgver"
)
b2sums=(
  '219ea3c0fa4884e17af74fad70cf5a6c303a93684101fbd3216c1d07801887ed087add45c93223330eb27da5cf3a9ec659fee72b6e83bb2ce0789cc9c6cc33ed'
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  pytest
}

package() {
  local \
    _site_packages
  _site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # Symlink license file
  install \
    -dm755 \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_Pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
