# Maintainer: Null Talisman <webmaster at raspii dot tech>
# Contributor: Shiroko <hhx.xxm at gmail.com>
# Contributor: Johnpoint <me at lvcshu.com>
# Contributor: sukanka <su975853527 [AT] gmail.com>
pkgname=clash-for-windows-bin
pkgver=0.20.6
pkgrel=1
pkgdesc="A Windows/macOS/Linux GUI based on Clash and Electron."
arch=("x86_64")
url="https://github.com/Fndroid/clash_for_windows_pkg"
depends=('electron19')
optdepends=(
    'nftables: Required for TUN mode.'
    'iproute2: Required for TUN mode.'
)
makedepends=('asar' 'npm')
source=(
    "${url}/releases/download/${pkgver}/Clash.for.Windows-${pkgver}-x64-linux.tar.gz"
    "clash-for-windows.desktop"
    "cfw"
)
sha256sums=('9d82d955f42044bd1db4e9bd1b2b69372d8d5ab65c5e84ae67aa121b1fd1ee94'
            '7fff6aad3b8bf9ec9202739cd64c2d6b39de886911eb70c20499e4c8f4573a0f'
            'fc0818c884052d0fdc83233fb5c26741aa4daa42d848979936a6e5a8217c676b')
options=(!strip)

build() {
    cd "Clash for Windows-${pkgver}-x64-linux"/resources/
    asar e app.asar apps
    cd apps
    # fix for autostart and system electron
    sed -i 's|r=n\[1\],|r="cfw\\nIcon=clash\\n",|g' dist/electron/renderer.js
    # hide ads
    sed -i 's|e._v("Advertisement"|e._v(""|g' dist/electron/renderer.js
    sed -i 's|staticClass:"ad-img-list"|staticClass:""|g' dist/electron/renderer.js
    sed -i 's|staticClass:"ad-img"|staticClass:""|g' dist/electron/renderer.js
    sed -i 's|"lazy-image-view"|""|g' dist/electron/renderer.js
    # rebuild
    sed -i 's|"electron-log": "^4.1.0",|"electron-log": "^4.4.6",|g' package.json
    rm -rf node_modules && HOME=${srcdir} npm install
    cd ..
    asar p apps cfw.asar
}

package() {
    cd "Clash for Windows-${pkgver}-x64-linux"
    install -Dm644 resources/cfw.asar -t ${pkgdir}/usr/share/${pkgname%-bin}
    cp -a --no-preserve=ownership resources/static ${pkgdir}/usr/share/${pkgname%-bin}/
    install -Dm755 ../cfw -t ${pkgdir}/usr/bin
    install -Dm644 ../clash-for-windows.desktop -t ${pkgdir}/usr/share/applications
    install -Dm644 resources/apps/dist/electron/static/imgs/icon_512.png ${pkgdir}/usr/share/icons/hicolor/512x512/apps/clash.png
}
