# $OpenBSD: Makefile,v 1.38 2013/10/03 16:37:16 dcoppa Exp $

COMMENT=		fast array and numeric programming library for Python
BROKEN-m88k=		lacks cpu information

MODPY_EGG_VERSION=	1.8.0
DISTNAME=		numpy-${MODPY_EGG_VERSION}
PKGNAME=		py-${DISTNAME}
CATEGORIES=		math devel

HOMEPAGE=		http://www.numpy.org/

# BSD
PERMIT_PACKAGE_CDROM=	Yes

WANTLIB=		blas m lapack ${MODFORTRAN_WANTLIB} ${MODPY_WANTLIB} \
			pthread

MASTER_SITES=		${MASTER_SITE_SOURCEFORGE:=numpy/}

MODULES=		lang/python \
			fortran

MODPY_SETUPTOOLS=	Yes
MODPY_SETUP=		setupegg.py

TEST_DEPENDS=	devel/py-nose

MODFORTRAN_COMPILER =	g77

BUILD_DEPENDS = ${MODFORTRAN_BUILD_DEPENDS}
LIB_DEPENDS=            math/lapack \
			${MODFORTRAN_LIB_DEPENDS}

# XXX Segmentation fault on powerpc when building py-scipy:
# remove '-funroll-loops'
.if ${MACHINE_ARCH} == "powerpc"
PATCH_LIST=	patch-* powerpc-patch-numpy_distutils_fcompiler_gnu_py
.endif

pre-build:
	cp -f ${WRKSRC}/numpy/distutils/site.cfg ${WRKSRC}/site.cfg

do-test: fake
	${MAKE_ENV} PYTHONPATH=${WRKINST}${MODPY_SITEPKG} ${MODPY_BIN} -c \
	    'import numpy ; numpy.test()'

.include <bsd.port.mk>
