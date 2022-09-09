# Maintainer: jmcb <joelsgp@protonmail.com>
pkgname=easyrp-git
pkgver=v3.0.r4.gbe829db
pkgrel=1
pkgdesc="Give yourself a Custom Rich Presence in Discord. "
arch=('x86_64')
url="https://github.com/Pizzabelly/EasyRP"
license=('MIT')
depends=()
makedepends=('git' 'cmake' 'meson' 'ninja')
optdepends=()
provides=('easyrp')
conflicts=('easyrp' 'easyrp-bin')
source=("${pkgname%-git}::git+https://github.com/Pizzabelly/EasyRP.git"
        'git+https://github.com/discord/discord-rpc.git#commit=7c41a8e')
sha256sums=('SKIP'
            'SKIP')

_sub='discord-rpc'


pkgver() {
  cd "${pkgname%-git}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}


# https://wiki.archlinux.org/title/VCS_package_guidelines#Git_submodules
prepare() {
  cd "${pkgname%-git}"
  git submodule init
  git config submodule.externals/vendor/${_sub}.url "${srcdir}/${_sub}"
  git submodule update
}

build() {
  cd "${pkgname%-git}"

  cd $_sub
  mkdir -p build
  cd build
  cmake .. -DENABLE_IO_THREAD=OFF
  make

  cd "${srcdir}"
  cd "${pkgname%-git}"
  meson --buildtype=release build
  ninja -C build
}

package() {
  cd "${pkgname%-git}"
  _dest="${pkgdir}/opt/${pkgname%-git}"
  install -D ${pkgname%-bin} "${_dest}/${pkgname%-bin}"
  install -Dm644 -t "${_dest}" config.ini readme.txt
}
