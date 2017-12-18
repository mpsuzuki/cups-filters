ACLOCAL_AMFLAGS = -I m4

AM_CPPFLAGS = -I$(top_srcdir)/oprs -I$(top_srcdir)/opvp -I$(top_srcdir)

pkgfilterdir = $(CUPS_SERVERBIN)/filter
pkgfilter_PROGRAMS = pdftoopvp

install-exec-hook:
	$(INSTALL) -d -m 755 $(DESTDIR)$(pkgfilterdir)

pkgfontconfigdir = $(sysconfdir)/$(FONTDIR)
pkgfontconfig_DATA = 99pdftoopvp.conf
EXTRA_DIST = 99pdftoopvp.conf

pdftoopvp_SOURCES = \
	oprs/OPRS.cxx \
	oprs/OPRS.h \
	oprs/OPVPSplashClip.cxx \
	oprs/OPVPSplashClip.h \
	oprs/OPVPSplash.cxx \
	oprs/OPVPSplash.h \
	oprs/OPVPSplashPath.cxx \
	oprs/OPVPSplashPath.h \
	oprs/OPVPSplashState.cxx \
	oprs/OPVPSplashState.h \
	oprs/OPVPSplashXPath.cxx \
	oprs/OPVPSplashXPath.h \
	oprs/OPVPWrapper.cxx \
	oprs/OPVPWrapper.h \
	oprs/OPVPWrapper_0_2.cxx \
	oprs/OPVPWrapper_0_2.h \
	OPVPError.h \
	opvp/opvp_common.h \
	opvp/opvp.h \
	opvp/opvp_0_2_0.h \
	OPVPOutputDev.cxx \
	OPVPOutputDev.h \
	pdftoopvp.cxx
pdftoopvp_CFLAGS = \
	$(CUPS_CFLAGS) \
	$(FREETYPE_CFLAGS) \
	$(FONTCONFIG_CFLAGS) \
	$(LIBPNG_CFLAGS) \
	$(POPPLER_CFLAGS) \
	-I$(srcdir)/oprs \
	-I$(srcdir)/opvp
pdftoopvp_CXXFLAGS = $(pdftoopvp_CFLAGS)
pdftoopvp_LDADD = \
	$(CUPS_LIBS) \
	$(FREETYPE_LIBS) \
	$(FONTCONFIG_LIBS) \
	$(LIBPNG_LIBS) \
	$(POPPLER_LIBS) \
	$(DLOPEN_LIBS)
