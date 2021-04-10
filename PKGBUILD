# Maintainer : Daniel Bermond <dbermond@archlinux.org>
# Contributor: Thomas Schneider <maxmusterm@gmail.com>

pkgname=svt-av1-git
pkgver=0.8.6.r90.gabb32d07
pkgrel=1
pkgdesc='Scalable Video Technology AV1 encoder and decoder (git version)'
arch=('x86_64')
url='https://gitlab.com/AOMediaCodec/SVT-AV1/'
license=('BSD' 'custom: Alliance for Open Media Patent License 1.0')
depends=('glibc')
makedepends=('git' 'cmake' 'yasm')
provides=('svt-av1')
conflicts=('svt-av1')
source=('git+https://gitlab.com/AOMediaCodec/SVT-AV1.git')
sha256sums=('SKIP')

pkgver() {
    git -C SVT-AV1 describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//'
}

build() {
    export CC=clang CXX=clang++ AR=llvm-ar AS=llvm-as STRIP=llvm-strip
    export CFLAGS='-march=native'
    export CXXFLAGS=$CFLAGS
    export LDFLAGS="$CFLAGS -fuse-ld=lld"
    export LDFLAGS+=' -Wl,-z,noexecstack'
    cd SVT-AV1/Build/linux
    ./build.sh -p /usr --enable-avx512 --enable-lto
}

package() {
    cd SVT-AV1/Build/linux/Release
    make DESTDIR="$pkgdir" install
    cd "$srcdir"
    install -D -m644 SVT-AV1/{LICENSE,PATENTS}.md -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
