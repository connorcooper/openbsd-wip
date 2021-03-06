# $OpenBSD: Makefile,v 1.47 2012/04/01 19:38:57 sthen Exp $

COMMENT-main=		service daemon that monitors one or more GPSes
COMMENT-x11=		GUI test apps using gpsd
COMMENT-php=		web-based gpsd monitor in php

VERSION=		3.9
DISTNAME=		gpsd-${VERSION}
PKGNAME-main=		gpsd-${VERSION}
PKGNAME-x11=		gpsd-x11-${VERSION}
PKGNAME-php=		gpsd-php-${VERSION}

# libgpsd version always equal to libgps version, thanks to scons code.
SHARED_LIBS += 		gps	20.0 # 20.0
SHARED_LIBS += 		gpsd	20.0 # 20.0
CATEGORIES=		misc geo

HOMEPAGE = 		http://www.catb.org/gpsd/

MAINTAINER=		Chris Kuethe <ckuethe@openbsd.org>

# BSD
PERMIT_PACKAGE_CDROM=	Yes

MASTER_SITES = 		${MASTER_SITE_SAVANNAH:=gpsd/}

MODULES += 		devel/scons lang/python lang/php
MODPHP_BUILDDEP=	No
MODSCONS_FLAGS += 	gpsd_user=_gpsd \
			libQgpsmm=no \
			mandir=man \
			pps=no \
			prefix=${PREFIX} \
			chrpath=no \
			shared=yes

MULTI_PACKAGES = 	-main -php -x11

BUILD_DEPENDS=		textproc/libxslt \
			textproc/docbook \
			textproc/docbook-xsl
TEST_DEPENDS=		misc/gpsd,-main

LIB_DEPENDS-main = 	devel/libusb1
RUN_DEPENDS-main=	${MODPY_RUN_DEPENDS}

RUN_DEPENDS-x11 = 	${BASE_PKGPATH} \
			devel/desktop-file-utils \
			x11/py-gtk2

WANTLIB-main = 		m ncurses c pthread stdc++ usb-1.0

# the php display script doesn't depend on having any of gpsd around,
# it just sits there and looks pretty.
PKG_ARCH-php=		*
RUN_DEPENDS-php=	lang/php/${MODPHP_VERSION},-gd
PREFIX-php=		/var/www/

SEPARATE_BUILD = 	Yes

SUBST_VARS += 		VERSION

MODPY_ADJ_FILES = 	gegps gpsprof gps/gps.py gpsfake gpscat xgps xgpsspeed \
			leapsecond.py test_maidenhead.py
pre-configure:
	@perl -pi -e "s,/usr/share/gpsd/,," ${WRKSRC}/packaging/X11/*.desktop
	@perl -pi -e "s,http://docbook.sourceforge.net/release/xsl/current/,${LOCALBASE}/share/xsl/docbook/," \
		${WRKSRC}/SConstruct
	@perl -pi -e "s,python,${MODPY_BIN}," ${WRKSRC}/regress-driver
	${SUBST_CMD} ${WRKSRC}/SConstruct

post-install:
	${INSTALL_DATA_DIR} ${WRKINST}/var/www/gpsd
	${INSTALL_SCRIPT} ${WRKSRC}/gpsd.php ${WRKINST}/var/www/gpsd
	${INSTALL_DATA} ${WRKSRC}/gpsd_config.h ${PREFIX}/include
	${INSTALL_DATA_DIR} ${PREFIX}/share/applications/
	${INSTALL_DATA_DIR} ${PREFIX}/share/pixmaps/
	${INSTALL_DATA} ${WRKSRC}/packaging/X11/*.desktop \
		${PREFIX}/share/applications/
	${INSTALL_DATA} ${WRKSRC}/packaging/X11/gpsd-logo.png \
		${PREFIX}/share/pixmaps/

# Generate .py[co] files for installed modules
# if that's not done, ${MODPY_SITEPKG}/gps will be polluted
# with these files when module is imported from root user.
# Taken from FreeBSD.
	@find ${DESTDIR}/${MODPY_SITEPKG}/gps -name "*.py" -exec \
		${MODPY_BIN} ${MODPY_LIBDIR}/py_compile.py {} \;
	@find ${DESTDIR}/${MODPY_SITEPKG}/gps -name "*.py" -exec \
		${MODPY_BIN} -O ${MODPY_LIBDIR}/py_compile.py {} \;

# Disable regression tests until they are fixed.
NO_TEST = 	Yes
do-test:
	@${SETENV} ${MAKE_ENV} ${MODSCONS_BIN} -C ${WRKSRC} \
		${MODSCONS_ENV} ${MODSCONS_FLAGS} check

.include <bsd.port.mk>
