# $NetBSD$

DISTNAME=	palemoon-27.6.2
CATEGORIES=	www
MASTER_SITES=	ftp://source:current@ftp2.palemoon.org/
EXTRACT_SUFX=   .7z
DISTFILES=	palemoon-27.6.2-source${EXTRACT_SUFX}

MAINTAINER=	INSERT_YOUR_MAIL_ADDRESS_HERE
HOMEPAGE=	ftp://source:current@ftp2.palemoon.org/
COMMENT=	TODO: Short description of the package
#LICENSE=	# TODO: (see mk/license.mk)

WRKSRC=		${WRKDIR}/palemoon
EXTRACT_DIR=    ${WRKSRC}

USE_TOOLS+=		pkg-config autoconf213 gmake perl python27 zip
USE_PKGLOCALEDIR=	yes
USE_LANGUAGES=		c99 c++
GNU_CONFIGURE=		YES


.if ${MACHINE_ARCH} == "i386" || ${MACHINE_ARCH} == "x86_64"
BUILD_DEPENDS+=         yasm>=1.1:../../devel/yasm
.endif

.if ${MACHINE_ARCH} == "i386"
CXXFLAGS+=              -march=i586
CXXFLAGS+=              -mstackrealign
.endif

#PKGCONFIG_OVERRIDE+=	gfx/harfbuzz/src/harfbuzz-gobject.pc.in
#PKGCONFIG_OVERRIDE+=	gfx/harfbuzz/src/harfbuzz-icu.pc.in
PKGCONFIG_OVERRIDE+=	gfx/harfbuzz/src/harfbuzz.pc.in
PKGCONFIG_OVERRIDE+=	intl/icu/source/config/icu.pc.in
PKGCONFIG_OVERRIDE+=	ipc/chromium/src/third_party/libevent/libevent.pc.in
PKGCONFIG_OVERRIDE+=	ipc/chromium/src/third_party/libevent/libevent_openssl.pc.in
PKGCONFIG_OVERRIDE+=	ipc/chromium/src/third_party/libevent/libevent_pthreads.pc.in
PKGCONFIG_OVERRIDE+=	js/src/ctypes/libffi/libffi.pc.in
PKGCONFIG_OVERRIDE+=	js/src/js.pc.in
PKGCONFIG_OVERRIDE+=	memory/jemalloc/src/jemalloc.pc.in
PKGCONFIG_OVERRIDE+=	nsprpub/config/nspr.pc.in
PKGCONFIG_OVERRIDE+=	security/nss/pkg/pkg-config/nss.pc.in
PKGCONFIG_OVERRIDE+=	xulrunner/installer/libxul-embedding.pc.in
PKGCONFIG_OVERRIDE+=	xulrunner/installer/libxul.pc.in
PKGCONFIG_OVERRIDE+=	xulrunner/installer/mozilla-js.pc.in
PKGCONFIG_OVERRIDE+=	xulrunner/installer/mozilla-nspr.pc.in
#PKGCONFIG_OVERRIDE+=	xulrunner/installer/mozilla-nss.pc.in
#PKGCONFIG_OVERRIDE+=	xulrunner/installer/mozilla-plugin.pc.in

CONFIGURE_ARGS+= --enable-application=browser
CONFIGURE_ARGS+= --with-pthreads
CONFIGURE_ARGS+= --disable-installer
CONFIGURE_ARGS+= --disable-updater
CONFIGURE_ARGS+= --enable-devtools
CONFIGURE_ARGS+= --enable-shared-js
CONFIGURE_ARGS+= --enable-pulseaudio
CONFIGURE_ARGS+= --enable-release
CONFIGURE_ARGS+= --disable-tests
CONFIGURE_ARGS+= --with-system-icu
CONFIGURE_ARGS+= --prefix=${PREFIX}

pre-configure: 
	chmod 755 -R ${WRKSRC}
	cd ${WRKSRC} && autoconf
	cd ${WRKSRC}/js/src && autoconf

do-configure:
	mkdir ${WRKDIR}/build \
	&& cd ${WRKDIR}/build \
	&& env ${CONFIGURE_ENV} ../palemoon/configure ${CONFIGURE_ARGS}

do-build:
	cd ${WRKDIR}/build \
	&& env ${MAKE_ENV} \
            ${MAKE_PROGRAM} ${BUILD_MAKE_FLAGS} ${_MAKE_JOBS} \
                -f ${MAKE_FILE}
	
do-install:
	cd ${WRKDIR}/build \
	&& env ${MAKE_ENV} \
		${MAKE_PROGRAM} ${INSTALL_MAKE_FLAGS} \
		-f ${MAKE_FILE} \
		${INSTALL_TARGET}

.include "../../textproc/icu/buildlink3.mk"
.include "../../x11/gtk2/buildlink3.mk"
.include "../../sysutils/dbus-glib/buildlink3.mk"
.include "../../audio/pulseaudio/buildlink3.mk"
.include "../../mk/bsd.pkg.mk"
