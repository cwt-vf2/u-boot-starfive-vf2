# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=u-boot-starfive-vf2
pkgver=2025.10
pkgrel=1
pkgdesc='U-Boot for StarFive RISC-V VisionFive 2 Board'
_tag=v${pkgver}
_srcname=u-boot-$pkgver
url="https://github.com/u-boot/u-boot/"
arch=(riscv64)
license=('GPL-2.0+')
makedepends=(gcc swig opensbi)
options=('!strip')
source=("${url}archive/refs/tags/${_tag}.tar.gz"
        config)

b2sums=('e215c58756e17213e44cacf938c9aa4b74d7657f0921970f06ad0c9103b5b5f5a8cae82e95042fb1ee5d0098f46eb64463a2184dd86f00cb303df5d3f23b4d58'
        '0c2bc118250331163a24f2a1d0389549e9178442b2ffe38ccb0a127e58e98661ed57d0f4275a5751974263ba8f6bbccf90b3d560ad74ca22b0dab760998e960e')


prepare() {
  cd $_srcname

  unset CFLAGS
  if command -v ccache 2>&1 >/dev/null; then 
    CCACHE=$(which ccache 2>/dev/null)
    gcc="${CCACHE} ${CROSS_COMPILE:-}gcc"
  else
    gcc="${CROSS_COMPILE:-}gcc"
  fi

  cp ../config .config
  make -j $(nproc) \
    CC="${gcc} -mcpu=sifive-u74 -mtune=sifive-7-series" \
    oldconfig
  cp .config ../../config.new
}

build() {
  cd $_srcname

  unset CFLAGS
  if command -v ccache 2>&1 >/dev/null; then 
    CCACHE=$(which ccache 2>/dev/null)
    gcc="${CCACHE} ${CROSS_COMPILE:-}gcc"
  else
    gcc="${CROSS_COMPILE:-}gcc"
  fi

  make -j $(nproc) \
    CC="${gcc} -mcpu=sifive-u74 -mtune=sifive-7-series" \
    OPENSBI=/usr/share/opensbi/lp64/generic/firmware/fw_dynamic.bin
}

package() {
  cd $_srcname
  install -Dm644 spl/u-boot-spl.bin.normal.out "$pkgdir/usr/share/$pkgname/u-boot-spl.bin.normal.out"
  install -Dm644 u-boot.itb "$pkgdir/usr/share/$pkgname/u-boot.itb"
}

# vim:set ts=8 sts=2 sw=2 et:
