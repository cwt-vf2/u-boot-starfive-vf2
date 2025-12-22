# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=u-boot-starfive-vf2
pkgver=2025.10
pkgrel=2
pkgdesc='U-Boot for StarFive RISC-V VisionFive 2 Board'
_tag=v${pkgver}
_srcname=u-boot-$pkgver
url="https://github.com/u-boot/u-boot/"
arch=(riscv64)
license=('GPL-2.0+')
makedepends=(gcc swig opensbi-git)
options=('!strip')
source=("${url}archive/refs/tags/${_tag}.tar.gz"
        config
        01-Add-support-for-StarFive-VisionFive-2-Lite-board.patch)

b2sums=('e215c58756e17213e44cacf938c9aa4b74d7657f0921970f06ad0c9103b5b5f5a8cae82e95042fb1ee5d0098f46eb64463a2184dd86f00cb303df5d3f23b4d58'
        '85e764306d18b9ee4737ab5d42667e1719aafbfb33ea76fa50fef5dcfec7d59a59d0265592e097c00533b5cdd66d06a9bc5e2ee6bba777b646ecf56ac1be2f7e'
        '6f1fe3372f140d9fb17a976176d175ede626d40c1dc9f38662a477839aa20011e7b002fb4ad455b754f19de271e34754d83aff0510ff77cd843980934aadcb15')


prepare() {
  cd $_srcname

  local src
  for src in $(ls ../*.patch); do
    echo "Applying patch $src..."
    patch -Np1 -F3 <"../$src"
  done

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
