# $OpenBSD $

COMMENT=	start an application as a daemon

MODULES=	cpan
DISTNAME=	App-Daemon-0.20
PKGNAME=	p5-${DISTNAME:S/-perl-/-/}
CATEGORIES=	devel

MAINTAINER =    Sergey Bronnikov <estetus@gmail.com>

# Perl
PERMIT_PACKAGE_CDROM=	Yes

RUN_DEPENDS=   	devel/p5-Proc-PID-File \
		devel/p5-Sysadm-Install

BUILD_DEPENDS=  ${RUN_DEPENDS}

.include <bsd.port.mk>
