# Maintainer: Miran Kljun <miran.kljun@gmail.com>
pkgname=rclone-wiz
pkgver=1.7.4
pkgrel=14
pkgdesc="A simple and easy to use tool to configure, script, and mount cloud drives using rclone"
arch=('x86_64' 'aarch64' 'armv7h')
url="https://github.com/themix88/Clone-WIZ"
license=('GPL3')

_rclone_ver=1.75.0

depends=(
    'python'
    'python-pyqt6'
    'fuse3'
)

makedepends=(
    'go'
    'git'
)

optdepends=(
    'konsole: Supported terminal for rclone config'
    'alacritty: Supported terminal for rclone config'
    'gnome-terminal: Supported terminal for rclone config'
    'xterm: Supported terminal for rclone config'
    'kitty: Supported terminal for rclone config'
    'ghostty: Supported terminal for rclone config'
)

_tag="v${pkgver}"
_baseurl="https://raw.githubusercontent.com/themix88/Clone-WIZ/${_tag}"

source=(
    "rclone-wiz.py::${_baseurl}/rclone-wiz.py"
    "rclone-wiz.desktop::${_baseurl}/rclone-wiz.desktop"
    "rclone-wiz.svg::${_baseurl}/rclone-wiz.svg"
    "LICENSE::${_baseurl}/LICENSE"
    "VERSION::${_baseurl}/VERSION"
    "README.md::${_baseurl}/README.md"
    "rclone::git+https://github.com/rclone/rclone.git#tag=v${_rclone_ver}"
)
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

build() {
    cd "$srcdir/rclone"
    export GOPATH="$srcdir/gopath"
    export CGO_ENABLED=0
    go build -trimpath \
        -ldflags "-s -w -X github.com/rclone/rclone/fs.Version=v${_rclone_ver}" \
        -o rclone \
        .
}

package() {
    # 1. Install the rclone binary built from source
    install -Dm755 "$srcdir/rclone/rclone" "$pkgdir/usr/bin/rclone"

    # 2. Install the executable
    install -Dm755 "$srcdir/rclone-wiz.py" "$pkgdir/usr/bin/rclone-wiz"

    # 3. Install the desktop file
    install -Dm644 "$srcdir/rclone-wiz.desktop" "$pkgdir/usr/share/applications/rclone-wiz.desktop"

    # 4. Install the icon to a standard directory
    install -Dm644 "$srcdir/rclone-wiz.svg" "$pkgdir/usr/share/pixmaps/rclone-wiz.svg"

    # 5. Copy license file GPL3
    install -Dm644 "$srcdir/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

    # 6. Put VERSION file to the application's shared data directory
    install -Dm644 "$srcdir/VERSION" "$pkgdir/usr/share/$pkgname/VERSION"

    # 7. Put README file to the application's shared data directory
    install -Dm644 "$srcdir/README.md" "$pkgdir/usr/share/$pkgname/README.md"
}