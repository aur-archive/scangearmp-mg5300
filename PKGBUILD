# Maintainer: Giancarlo Bianchi <giancarlobianchi76@gmail.com>
# Original Contributors: Fortunato Ventre (voRia) <vorione@gmail.com>
# Custom Processing Unlimited (CPUnltd) <CPUnltd 'at' gmail 'dot' com>
pkgname=scangearmp-mg5300
pkgver=1.80
pkgrel=8
_pkgver=1.80-1
pkgdesc="Canon Scanner Driver (for MG5300 series)"
url="http://support-my.canon-asia.com/contents/MY/EN/0100393102.html"
arch=('i686' 'x86_64')
license=('custom')
depends=('gimp' 'sane' 'libpng')
source=(http://gdlp01.c-wss.com/gds/1/0100003931/01/scangearmp-source-${_pkgver}.tar.gz
        'build-fixes.patch'
				'libpng.patch')
md5sums=('88e3891918357304a9f527d043b435d2'
         '9aed5d96ede0606b58a51a3e73ec2f20'
         '1a4339b7d09af7b1a8a40abb791433d7')

build() {
	if [ "$CARCH" == "x86_64" ]; then  
	  libdir=libs_bin64
	else
	  libdir=libs_bin32
	fi

	# Build fixes
	patch -p0 < build-fixes.patch
	patch -p0 < libpng.patch

	cd ${srcdir}/scangearmp-source-${_pkgver}/scangearmp
	./autogen.sh --prefix=/usr LDFLAGS="-L`pwd`/../com/${libdir} -lm"
	# Force the use of system's libtool
	rm -f libtool
	ln -s `which libtool` .
	# Build package
	make clean
	make
	# Install package
	install -d -m 0755 $pkgdir/usr/lib/bjlib
	make DESTDIR=${pkgdir} install

	# Install SANE configuration file
	install -d -m 0755 $pkgdir/etc/sane.d/
	install -m 0644 ${srcdir}/scangearmp-source-${_pkgver}/scangearmp/backend/canon_mfp.conf $pkgdir/etc/sane.d/canon_mfp.conf

	# Install common libraries
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/com/${libdir}/libcncpcmcm.so.8.0.1 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/com/${libdir}/libcncpmsimg.so.1.0.2 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/com/${libdir}/libcncpmslld.so.1.0.1 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/com/${libdir}/libcncpmsui.so.1.8.0 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/com/${libdir}/libcncpnet.so.1.2.2 ${pkgdir}/usr/lib/
	# Install mg5300 series specific libraries
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/${libdir}/libcncpmsimg389.so.1.8.0 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/${libdir}/libcncpmslld389c.so.1.04.1 ${pkgdir}/usr/lib/
	install -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/${libdir}/libcncpmslld389.so.1.8.0 ${pkgdir}/usr/lib/
	# Create symbolic links
	cd ${pkgdir}/usr/lib/
	ln -s libcncpcmcm.so.8.0.1 libcncpcmcm.so
	ln -s libcncpmsimg.so.1.0.2 libcncpmsimg.so
	ln -s libcncpmslld.so.1.0.1 libcncpmslld.so
	ln -s libcncpmsui.so.1.8.0 libcncpmsui.so
	ln -s libcncpnet.so.1.2.2 libcncpnet.so

	ln -s libcncpmsimg389.so.1.8.0 libcncpmsimg389.so
	ln -s libcncpmslld389c.so.1.04.1 libcncpmslld389c.so
	ln -s libcncpmslld389.so.1.8.0 libcncpmslld389.so
	
	# Make scangearmp usable from gimp
	install -d -m 0755 ${pkgdir}/usr/lib/gimp/2.0/plug-ins/
	ln -s /usr/bin/scangearmp ${pkgdir}/usr/lib/gimp/2.0/plug-ins/

	# Install .tbl and .dat files for mg5300 series
	install -D -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/cnc1754d.tbl ${pkgdir}/usr/lib/bjlib/
	install -D -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/cnc_3890.tbl ${pkgdir}/usr/lib/bjlib/
	install -D -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/CNC_389H.DAT ${pkgdir}/usr/lib/bjlib/
	install -D -m 0755 ${srcdir}/scangearmp-source-${_pkgver}/389/CNC_389P.DAT ${pkgdir}/usr/lib/bjlib/

	# Install udev rules
	install -D -m 0644 ${srcdir}/scangearmp-source-${_pkgver}/scangearmp/etc/80-canon_mfp.rules ${pkgdir}/etc/udev/rules.d/80-canon_mfp.rules

	# Install .ini file
	install -D -m 0666 ${srcdir}/scangearmp-source-${_pkgver}/com/ini/canon_mfp_net.ini ${pkgdir}/usr/lib/bjlib/

	# Install license file
	cd ${srcdir}/scangearmp-source-${_pkgver}
	install -D LICENSE-scangearmp-${pkgver}EN.txt ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE-scangearmp-${pkgver}EN.txt

	# Remove unneeded files
	rm ${pkgdir}/usr/lib/libsane-canon_mfp.a
	rm ${pkgdir}/usr/lib/libsane-canon_mfp.la
}

# vim:set ts=2 sw=2 :et
