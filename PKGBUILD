# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=u-boot-starfive-vf2
pkgver=2025.01
pkgrel=1
pkgdesc='U-Boot for StarFive RISC-V VisionFive 2 Board'
_tag=v${pkgver}
_srcname=u-boot-$pkgver
url="https://github.com/u-boot/u-boot/"
arch=(riscv64)
license=('GPL-2.0+')
makedepends=(gcc swig sed opensbi)
options=('!strip')
source=("${url}archive/refs/tags/${_tag}.tar.gz"
        'config-btrfs.patch'
        'config-zbb.patch'
        'config-ipv6.patch'
        'config-link_local.patch'
        'config-rtc.patch'
        'config-starfive_timer.patch')

b2sums=('206183e2181131f3dd2dbdc682fc71e449038e42a64096f48c7dc08d615f102f7b41a97c58f0e92c98996c60fb4e2e287c2f86f04ebd796e29b7404c631c2200'
        '32b8d1e6a815a153e3a740a9eced49dcea99548567fe8ae2f697bb89e11e6f834205b15d504237d84bda09b171a97ecf7a3307eded1fc2254d784e0a0db144d0'
        '49c26037162956870b681c6d976e7554703e8fbf64171f8dc89b1288dc8db25829944156e03e3a2110cbd7bd7c399d1a3062033a2a6b2ff83184da4af9447e0a'
        'bb246e391679b298ca965a14bd229438bb17187232279cd35ec28c9e3aa465e3a336ff78861ceb671c157518aefcbd381aead8b99893c0f14255d3916d44d7d8'
        'ea9076b4fb1cef545324d22c338866e65b640b49af2391ac2c78c6ace97bdd5959635e6d976f7f06c277c381cac925193e230c17f71cf8350370a639349e06f0'
        '1a6470492e34b6c9bf0ad115c29f05c18a3f15ee6ee87e1dc1932c0fdd14081e057910a1de1294f996049f5585929afd8e80442f32e84a156c76e89226da85b1'
        '1ec62523fd7f61075a97732ba93d8b66366a67ec07b97492cca617ede0a044b23c2cb453e0a1fbf63a6bea8fcbfc1cfdbda6b4daba4156aa5961edf0210ff452')


prepare() {
  cd $_srcname
  make -j $(nproc) \
    CC="gcc -mcpu=sifive-u74 -mtune=sifive-7-series" \
    starfive_visionfive2_defconfig
  sed -i -e "s/^# CONFIG_CMD_SYSBOOT is not set$/CONFIG_CMD_SYSBOOT=y/" .config
  patch -p0 < ../config-btrfs.patch
  patch -p0 < ../config-zbb.patch
  patch -p0 < ../config-ipv6.patch
  patch -p0 < ../config-link_local.patch
  patch -p0 < ../config-rtc.patch
  patch -p0 < ../config-starfive_timer.patch
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
