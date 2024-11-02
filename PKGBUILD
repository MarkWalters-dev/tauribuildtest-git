# Maintainer: Mark Walters <aur-github@MarkWalters.dev>

_pkgname="tauribuildtest"
_pkglower=${_pkgname,,}
pkgname="$_pkglower-git"
pkgver=0.0.1
pkgrel=1
pkgdesc="A Tauri App"
arch=("x86_64")
url="https://github.com/MarkWalters-dev/$_pkgname"
license=("GPL")
depends=('webkit2gtk' 'gtk3')
conflicts=("$_pkglower-bin")
makedepends=('git' 'nodejs' 'pnpm' 'rust' 'cargo')
options=(!debug) 
#source=("git+$url"
source=("${_pkglower}-${pkgver}.tar.gz::${url}/archive/refs/tags/${pkgver}.tar.gz"
        "$_pkglower.desktop")
sha256sums=('8c8375faa6f78817898a318e383a7f828e367689b36d7da4528984b98421a5ab'
            '91a36873e97e6a61f4e6da69bb7497651e96f99de7beb5f3df886bef3dff9381')

build() {
  # Must move .gitignore for tauri to build. Tauri will never fix the intentional bug.
  # https://github.com/tauri-apps/tauri/issues/7427

  local base=$(dirname $srcdir)
  [ -f "$base/.gitignore" ] && mv "$base/.gitignore" "$base/.gitignore.bak"

  cd "$srcdir/$_pkgname-$pkgver"
  pnpm install
  pnpm tauri build --no-bundle
  [ -f "$base/.gitignore.bak" ] && mv "$base/.gitignore.bak" "$base/.gitignore"
}

package() {
  install -Dm644 "$_pkglower.desktop" "${pkgdir}/usr/share/applications/$_pkglower.desktop"
  install -Dm755 "$srcdir/$_pkgname-$pkgver/src-tauri/target/release/$_pkglower" "$pkgdir/usr/bin/$_pkglower"
}
