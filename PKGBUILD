# Maintainer: DarkXero

pkgname=xero-calamares-test
_pkgname=calamares
pkgver=3.3.0.230306
pkgrel=4
pkgdesc='Distribution-independent installer framework'
arch=('i686' 'x86_64')
license=(GPL)
url="https://github.com/calamares/calamares"
license=('LGPL')
depends=('kdbusaddons' 'kconfig' 'kcoreaddons' 'kiconthemes' 'ki18n' 'kio' 'solid' 'yaml-cpp' 'kpmcore' 'mkinitcpio-openswap'
         'boost-libs' 'ckbcomp' 'hwinfo' 'qt5-svg' 'polkit-qt5' 'gtk-update-icon-cache' 'plasma-framework'
         'qt5-xmlpatterns' 'squashfs-tools' 'libpwquality' 'efibootmgr' 'icu' 'appstream-qt')
makedepends=('extra-cmake-modules' 'qt5-tools' 'qt5-translations' 'git' 'boost' 'kparts' 'kdbusaddons')
backup=('usr/share/calamares/modules/bootloader.conf'
        'usr/share/calamares/modules/displaymanager.conf'
        'usr/share/calamares/modules/initcpio.conf'
        'usr/share/calamares/modules/unpackfs.conf')

source=($pkgname::git+https://github.com/calamares/calamares#commit=da295e0)
sha256sums=('SKIP')
#	"calamares.desktop"

prepare() {
	cd ${srcdir}/$pkgname
	sed -i -e 's/Install packages/Updating System (Might Take a While...)/' "$srcdir/${pkgname}/src/modules/packages/main.py"
	#sed -i 's/,\ self.line_cb//g'  "$srcdir/${_pkgname}-${pkgver}/src/modules/packages/main.py"

	# patches here

	# change version
	_ver="$(cat CMakeLists.txt | grep -m3 -e "  VERSION" | grep -o "[[:digit:]]*" | xargs | sed s'/ /./g')"
	#printf 'Version: %s-%s' "${_ver}" "${pkgrel}"
	sed -i -e "s|\${CALAMARES_VERSION_MAJOR}.\${CALAMARES_VERSION_MINOR}.\${CALAMARES_VERSION_PATCH}|${_ver}-${pkgrel}|g" CMakeLists.txt
	sed -i -e "s|CALAMARES_VERSION_RC 1|CALAMARES_VERSION_RC 0|g" CMakeLists.txt

}

build() {
	cd ${srcdir}/$pkgname

	mkdir -p build
	cd build
        cmake .. \
              -DCMAKE_BUILD_TYPE=Release \
              -DCMAKE_INSTALL_PREFIX=/usr \
              -DCMAKE_INSTALL_LIBDIR=lib \
              -DWITH_PYTHONQT=OFF \
              -DWITH_KF5DBus=OFF \
              -DBoost_NO_BOOST_CMAKE=ON \
              -DWEBVIEW_FORCE_WEBKIT=OFF \
              -DSKIP_MODULES="webview tracking webview interactiveterminal initramfs \
                              initramfscfg dracut dracutlukscfg \
                              dummyprocess dummypython dummycpp \
                              dummypythonqt services-openrc \
                              keyboardq localeq welcomeq"
        make
}

package() {
	cd ${srcdir}/$pkgname/build
	make DESTDIR="$pkgdir" install
	rm "${srcdir}/$pkgname/calamares.desktop"
	rm "$pkgdir/usr/share/applications/calamares.desktop"
	#install -Dm644 "../data/manjaro-icon.svg" "$pkgdir/usr/share/icons/hicolor/scalable/apps/calamares.svg"
	#install -Dm644 "../data/calamares.desktop" "$pkgdir/usr/share/applications/calamares.desktop"
	#install -Dm755 "../data/calamares_polkit" "$pkgdir/usr/bin/calamares_polkit"
	#install -Dm644 "../data/49-nopasswd-calamares.rules" "$pkgdir/etc/polkit-1/rules.d/49-nopasswd-calamares.rules"
	#chmod 750      "$pkgdir"/etc/polkit-1/rules.d

	# rename services-systemd back to services
	#mv "$pkgdir/usr/lib/calamares/modules/services-systemd" "$pkgdir/usr/lib/calamares/modules/services"
	#mv "$pkgdir/usr/share/calamares/modules/services-systemd.conf" "$pkgdir/usr/share/calamares/modules/services.conf"
	#sed -i -e 's/-systemd//' "$pkgdir/usr/lib/calamares/modules/services/module.desc"
	#sed -i -e 's/-systemd//' "$pkgdir/usr/share/calamares/settings.conf"
}
