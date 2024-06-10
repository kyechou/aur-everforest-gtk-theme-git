# Maintainer: Kuan-Yen Chou <kuanyenchou at gmail dot com>

pkgname=everforest-gtk-theme-git
pkgver=r37.32093119
pkgrel=1
pkgdesc='Everforest colour palette for GTK'
arch=('any')
url="https://github.com/Fausto-Korpsvart/Everforest-GTK-Theme"
license=('GPL3')
depends=('gtk-engine-murrine')
makedepends=('git' 'sassc')
source=("$pkgname"::'git+https://github.com/Fausto-Korpsvart/Everforest-GTK-Theme')
sha256sums=('SKIP')

pkgver() {
    cd "$srcdir/$pkgname"
    if git describe --long --tags >/dev/null 2>&1; then
        git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
    else
        printf 'r%s.%s' "$(git rev-list --count HEAD)" "$(git describe --always)"
    fi
}

prepare() {
    cd "$srcdir/$pkgname"
    sed -i icons/*/index.theme \
        -e 's/oomox-//'
    sed -i icons/everforest_light/index.theme \
        -e 's/[Ee]verforest_[Ll]ight/Everforest-Light/g'

    # This can be removed once the following issue is resolved.
    # https://github.com/Fausto-Korpsvart/Everforest-GTK-Theme/issues/14
    # https://github.com/Fausto-Korpsvart/Everforest-GTK-Theme/pull/15
    local lower_yellow_dark='themes/src/assets/gtk/thumbnails/thumbnail-Yellow-dark.png'
    local upper_yellow_dark='themes/src/assets/gtk/thumbnails/thumbnail-Yellow-Dark.png'
    if [[ -f "$lower_yellow_dark" ]] && [[ ! -f "$upper_yellow_dark" ]]; then
        mv "$lower_yellow_dark" "$upper_yellow_dark"
    fi
}

build() {
    cd "$srcdir/$pkgname/themes"
    ./build.sh
}

package() {
    cd "$srcdir/$pkgname"

    # Install icons
    mkdir -p "$pkgdir/usr/share/icons"
    cp -r icons/Everforest-Dark "$pkgdir/usr/share/icons/"
    cp -r icons/everforest_light "$pkgdir/usr/share/icons/Everforest-Light"

    # Install themes
    mkdir -p "$pkgdir/usr/share/themes"
    ./themes/install.sh --dest "$pkgdir/usr/share/themes" --theme all
}

# vim: set ts=4 sw=4 et :
