# Maintainer: Guoxin "7Ji" Pu <pugokushin@gmail.com>

_name_canonical=ExaGear
_pkgver='3.1.0'
_srcname="${_name_canonical}_${_pkgver}"
_product="${_name_canonical}_Server_for_Ubuntu20"
_deb_parent="${_srcname}/${_product}"
_deb_common="${_deb_parent}/release/${_name_canonical,,}-"
_ver_guest=2793
_ver_other=2952
_suffixes=({core-x{32,64}a64,guest,integration,utils})

pkgbase="${_name_canonical,,}"
pkgname=("${_suffixes[@]/#/"${pkgbase}"-}" "${pkgbase}-all")
pkgver="${_pkgver//-/_}"
pkgrel=1
pkgdesc="${_name_canonical}, an x86_64 to ARM translator by Huawei for Kunpeng"
url="https://www.hikunpeng.com/developer/devkit/download/${pkgbase}"
license=('custom')
source=(
  "https://mirrors.huaweicloud.com/kunpeng/archive/${_name_canonical}/${_srcname}.tar.gz"
)
noextract=(
  "${source[0]##*/}"
)
sha256sums=(
  '8b81e2f6bd87ce1010c48bb919429492b2136aae4e87f77b39101ff4c312ee54'
)
arch=('aarch64')
options=('!strip' '!debug')

prepare() {
  echo "Extracting needed debs..."
  tar -xf "${noextract[0]}" "${_deb_parent}"
}

_deb_to_pkgdir() {
  echo "Extracting deb file '${_deb}' into '${pkgdir}'..."
  bsdtar -xOf "${_deb}" ./data.tar.xz |
    xz -cdT0 |
    bsdtar -xpC "${pkgdir}"
}

_package-core-x32a64() {
  pkgdesc+=' (core translator to run 32-bit x86 programs on aarch64)'
  local _deb="${_deb_common}${_suffix}_${_ver_other}_arm64.deb"
  _deb_to_pkgdir
  mv "${pkgdir}/lib" "${pkgdir}/usr/"
}
_package-core-x64a64() {
  pkgdesc+=' (core translator to run 64-bit x86 programs on aarch64)'
  local _deb="${_deb_common}${_suffix}_${_ver_other}_arm64.deb"
  _deb_to_pkgdir
  mv "${pkgdir}/lib" "${pkgdir}/usr/"
}
_package-guest() {
  pkgdesc+=' (Ubuntu 20.04 (x86_64) guest image)'
  local _deb="${_deb_common}${_suffix}-for-ubuntu-2004-x86-64_${_ver_guest}_all.deb"
  _deb_to_pkgdir
}
_package-integration() {
  pkgdesc+=' (system integration)'
  local _deb="${_deb_common}${_suffix}_${_ver_other}_all.deb"
  _deb_to_pkgdir
  mv "${pkgdir}/lib" "${pkgdir}/usr/"
}
_package-utils() {
  pkgdesc+=' (utilities)'
  local _deb="${_deb_common}${_suffix}_${_ver_other}_all.deb"
  _deb_to_pkgdir
}
_package-all() {
  pkgdesc+=' (all)'
  depends=("${_suffixes[@]/#/"${pkgbase}"-}")
}

for _suffix in "${_suffixes[@]}" all; do
  eval "package_${pkgbase}-${_suffix}() {
    local _suffix="${_suffix}"
    $(declare -f "_package-${_suffix}")
    _package-${_suffix}
  }"
done
