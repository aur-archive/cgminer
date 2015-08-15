# Maintainer: Felix Yan <felixonmars@gmail.com>
# Contributor: monson <holymonson@gmail.com>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: David Manouchehri <david@davidmanouchehri.com>

pkgname=cgminer
pkgver=2.9.3
_build=
pkgrel=1
pkgdesc="Multi-threaded multi-pool CPU and GPU miner for bitcoin, forked from cpuminer."
url='http://forum.bitcoin.org/index.php?topic=28402.0'
license=('GPL3')
arch=('i686' 'x86_64')
depends=('ncurses' 'curl' 'libcl')
makedepends=('opencl-headers')
optdepends=('opencl-nvidia: OpenCL implementation for NVIDIA'   \
            'intel-opencl-sdk: OpenCL implementation for Intel' \
            'amdstream: OpenCL implementation for AMD')
source=("http://ck.kolivas.org/apps/cgminer/$pkgname-$pkgver.tar.bz2"
        "$pkgname.conf.d"
        "$pkgname"
        "$pkgname.service")
backup=("etc/conf.d/$pkgname" "etc/$pkgname.conf")

[ $CARCH == 'x86_64' ] && makedepends+=('yasm')

build() {
  cd "$srcdir"
  cd $pkgname-$pkgver${_build}

  # Use in-tree jansson since it is not compatible with jansson 2.0
  #sed -e 's/^AC_CHECK_LIB(jansson, json_loads, request_jansson=false, request_jansson=true)$/request_jansson=true/' -i configure.ac

  # Here you may want to use custom CFLAGS
  #export CFLAGS="-O2 -march=native -mtune=native -msse2"

  ./configure --prefix=/usr --enable-cpumining
  
  make
}

package() {
  cd "$srcdir"/$pkgname-$pkgver${_build}

  make DESTDIR="$pkgdir" install

  install -Dm755 "$srcdir"/$pkgname "$pkgdir"/etc/rc.d/$pkgname
  install -Dm644 "$srcdir"/$pkgname.service "$pkgdir"/usr/lib/systemd/system/$pkgname.service
  install -Dm644 "$srcdir"/$pkgname.conf.d "$pkgdir"/etc/conf.d/$pkgname
  sed 's#/usr/local/bin#/usr/bin#g' example.conf > $pkgname.conf
  install -Dm644 $pkgname.conf "$pkgdir"/etc/$pkgname.conf
}
md5sums=('f386036c72861f7b2c130de6ed990d33'
         'fe4a243fabe24608f5c05e40f0f118f6'
         'ee39698273671fee0e98d4af16014c36'
         'c2bb974adf92cc234fbf0136ebcb355d')
