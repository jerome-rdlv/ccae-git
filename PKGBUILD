# Maintainer: Jérôme Mulsant <jerome@rue-de-la-vieille.fr>
pkgname=ccae-git
pkgver=3.5.1.r5.g157f549
pkgrel=1
epoch=
pkgdesc="Colour Contrast Analyser (CCA) - Checks color contrast against WCAG criteria."
arch=('any')
url="https://developer.paciellogroup.com/color-contrast-checker/"
license=('GPL3')
makedepends=('git' 'npm' 'libxcrypt-compat')
source=("$pkgname::git+https://github.com/ThePacielloGroup/CCAe.git"
    "cca.desktop")
noextract=()
sha256sums=('SKIP'
            '573202ba311de756575dca1bf5d6469d352c1155afdd3347c79949d7cfcf053c')

pkgver() {
    cd $pkgname
    git describe --tags --long | sed 's/^v//;s/-/.r/;s/-/./'
}

build() {
    cd "$pkgname"
    npm install
    npx electron-builder build -l tar.xz
}

package() {
    depends=('hicolor-icon-theme')

    # Install desktop file
    install -Dm0644 cca.desktop -t "${pkgdir}"/usr/share/applications/

    cd "${srcdir}/${pkgname}/build"

    # Install icons
    resolutions=(16x16 32x32 48x48 64x64 96x96 128x128 256x256 512x512)
    for resolution in "${resolutions[@]}"
    do
        install -Dm0644 "${resolution}.png" \
            "${pkgdir}/usr/share/icons/hicolor/${resolution}/apps/cca.png"
    done

    install -Dm0644 "icon.svg" "${pkgdir}/usr/share/icons/hicolor/scalable/apps/cca.svg"

    cd "${srcdir}/${pkgname}/dist/linux-unpacked"

    install -dm0755 "${pkgdir}/opt/cca"
    cp -r ./* "${pkgdir}/opt/cca"

    # Symlink /usr/bin executable to opt
    install -dm0755 "${pkgdir}/usr/bin"
    ln -s /opt/cca/cca "${pkgdir}/usr/bin/cca"
}
