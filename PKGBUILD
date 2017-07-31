# Maintainer: emersion <contact emersion.fr>
pkgname=browserpass
pkgver=1.0.6
pkgrel=2
pkgdesc="Chrome & Firefox browser extension for pass, a UNIX password manager"
arch=('i686' 'x86_64')
url="https://github.com/tcarrio/browserpass"
license=('MIT')
depends=('pass')
makedepends=('go')
optdepends=()
source=("git+https://github.com/tcarrio/browserpass.git#branch=copy-pass-login-buttons")
md5sums=('SKIP')

build() {
	export GOPATH="$(pwd)/.go"

	go_pkgname="github.com/dannyvankooten/browserpass"
	go_pkgpath="$GOPATH/src/$go_pkgname"
	mkdir -p "$(dirname $go_pkgpath)"
	ln -s "$srcdir/$pkgname" "$go_pkgpath"

	cd "$srcdir/$pkgname"
	make browserpass static-files
}

package() {
	cd "$srcdir/$pkgname"

	install -D browserpass "$pkgdir/usr/bin/browserpass"

	host_file="/usr/bin/browserpass"
	escaped_host_file=${host_file////\\/}
	sed -i -e "s/%%replace%%/$escaped_host_file/" chrome-host.json
	sed -i -e "s/%%replace%%/$escaped_host_file/" firefox-host.json

	app_name="com.dannyvankooten.browserpass"

	install -D chrome-host.json "$pkgdir/etc/opt/chrome/native-messaging-hosts/$app_name.json"
	install -D chrome-policy.json "$pkgdir/etc/opt/chrome/policies/managed/$app_name.json"
	install -D chrome-host.json "$pkgdir/etc/chromium/native-messaging-hosts/$app_name.json"
	install -D chrome-policy.json "$pkgdir/etc/chromium/policies/managed/$app_name.json"

	install -D firefox-host.json "$pkgdir/usr/lib/mozilla/native-messaging-hosts/$app_name.json"
}
