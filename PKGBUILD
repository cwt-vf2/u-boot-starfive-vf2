# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=u-boot-starfive-vf2
pkgver=2024.07
pkgrel=3
pkgdesc='U-Boot for StarFive RISC-V VisionFive 2 Board'
_tag=v${pkgver}
_srcname=u-boot-$pkgver
url="https://github.com/u-boot/u-boot/"
arch=(riscv64)
license=('GPL-2.0+')
makedepends=(gcc swig sed opensbi-6.6-starfive-vf2)
options=('!strip')
source=("${url}archive/refs/tags/${_tag}.tar.gz")

b2sums=('66aa2c4152d0eb1115d21e4e03a648b178cc98e6d832f98561066dfa531f62cdc39745f1efdcc65dada95e83bca7cc9314e58984c9a5c0412ed4c1a3fd068a35')


prepare() {
  cd $_srcname
  make -j $(nproc) \
    CC="gcc -mcpu=sifive-u74 -mtune=sifive-7-series" \
    starfive_visionfive2_defconfig
  sed -i -e "s/^# CONFIG_CMD_SYSBOOT is not set$/CONFIG_CMD_SYSBOOT=y/" .config
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
