# Maintainer: DarkXero

pkgname=calamares-app
_pkgname=calamares
pkgver=3.3.0.230710
pkgrel=2
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

source=($pkgname::git+https://github.com/calamares/calamares#commit=11e1659
        "calamares_polkit")

prepare() {

	cp -rv ../modules/* ${srcdir}/$pkgname/src/modules/

	# patches here
	sed -i -e 's/"Install configuration files" OFF/"Install configuration files" ON/' "$srcdir/${pkgname}/CMakeLists.txt"
	sed -i -e 's/# DEBUG_FILESYSTEMS/DEBUG_FILESYSTEMS/' "$srcdir/${pkgname}/CMakeLists.txt"
	sed -i -e "s/desired_size = 512 \* 1024 \* 1024  \# 512MiB/desired_size = 512 \* 1024 \* 1024 \* 4  \# 2048MiB/" "$srcdir/${pkgname}/src/modules/fstab/main.py"

	# change version
	cd ${srcdir}/$pkgname/src/
	_ver="$(cat CMakeLists.txt | grep -m3 -e "  VERSION" | grep -o "[[:digit:]]*" | xargs | sed s'/ /./g')"
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
    install -Dm755 "../calamares_polkit" "$pkgdir/usr/bin/calamares_polkit"
	cd ${srcdir}/$pkgname/build
	make DESTDIR="$pkgdir" install
	rm "${srcdir}/$pkgname/calamares.desktop"
	rm "$pkgdir/usr/share/applications/calamares.desktop"

}
sha256sums=('SKIP'
            'e61245ff7e4c3af6f05a9fe9a3fcf47f2780d9aa88c11eab02a35fac446cf1e1')
