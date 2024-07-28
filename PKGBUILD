# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=u-boot-starfive-vf2
pkgver=2024.07
pkgrel=1
pkgdesc='U-Boot for StarFive RISC-V VisionFive 2 Board'
_tag=v${pkgver}
_srcname=u-boot-$pkgver
url="https://github.com/u-boot/u-boot/"
arch=(riscv64)
license=('GPL-2.0+')
makedepends=(gcc swig opensbi-6.6-starfive-vf2)
options=('!strip')
source=("${url}archive/refs/tags/${_tag}.tar.gz")

sha256sums=('b7f6137acc89e4a939075600de3a04cc3a8602fa936194c27bd9a14005bc61fd')


prepare() {
  cd $_srcname
  make -j $(nproc) \
    CC="gcc -mcpu=sifive-u74 -mtune=sifive-7-series" \
    starfive_visionfive2_defconfig
}

build() {
  cd $_srcname
  make -j $(nproc) \
    CC="gcc -mcpu=sifive-u74 -mtune=sifive-7-series" \
    OPENSBI=/usr/share/opensbi/lp64/generic/firmware/fw_dynamic.bin
}

package() {
  cd $_srcname
  install -Dm644 spl/u-boot-spl.bin.normal.out "$pkgdir/usr/share/$pkgname/u-boot-spl.bin.normal.out"
  install -Dm644 u-boot.itb "$pkgdir/usr/share/$pkgname/u-boot.itb"
}

# vim:set ts=8 sts=2 sw=2 et:
