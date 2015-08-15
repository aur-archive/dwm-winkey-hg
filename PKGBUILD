# Maintainer: Daniel YC Lin <dlin.tw at gmail>
# Contributor: Army <uli_armbruster at web.de>
# Contributor: v2punkt0 <v2punkt0 at gmail>

pkgname=dwm-winkey-hg
pkgver=1603
pkgrel=1
pkgdesc="a dynamic window manager for X with winkey patch"
url="http://dwm.suckless.org"
license=('custom:MIT/X')
arch=('i686' 'x86_64')
depends=('libxinerama' 'dmenu')
makedepends=('mercurial')
conflicts=('dwm')
provides=('dwm')
source=("winkey.patch")

_hgroot='http://code.suckless.org/hg'
_hgrepo='dwm'

build() {
	rm -rf $srcdir/$_hgrepo-build
	cp -r $srcdir/$_hgrepo $srcdir/$_hgrepo-build
	cd $srcdir/$_hgrepo-build

	# add correct settings to config.mk
	sed -i "s|^PREFIX =.*|PREFIX = /usr|" config.mk
	sed -i "s|^X11INC =.*|X11INC = /usr/include/X11|" config.mk
	sed -i "s|^X11LIB =.*|X11LIB = /usr/lib/X11|" config.mk

	# for custom patches
	if ls $startdir | grep .*\.patch$
	then
		for i in $startdir/*\.patch
		do
			msg "patching with $i"
			patch -i $i
		done
	fi

	# for custom configs
	if test -e $startdir/config.h
	then
		msg "use custom config.h"
		cp ${startdir}/config.h .
	else
		msg "use default config.h"
	fi

	# finally start to make this thing!
	make
}

package() {
	cd $srcdir/$_hgrepo-build
	make PREFIX=$pkgdir/usr install
	install -Dm644 "$srcdir/$_hgrepo-build/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
sha1sums=('626bb8f28c9b3c391159488ea45a97b9d753d2f9')
