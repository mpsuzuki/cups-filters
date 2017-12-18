# Process this file with autoconf to create configure.

AC_PREREQ([2.65])

# ====================
# Version informations
# ====================
m4_define([pdftoopvp_version_major],[1])
m4_define([pdftoopvp_version_minor],[17])
m4_define([pdftoopvp_version_micro],[9])
m4_define([pdftoopvp_version],[pdftoopvp_version_major.pdftoopvp_version_minor.pdftoopvp_version_micro])

# =============
# Automake init
# =============
AC_INIT([cups-filter-opvp],[pdftoopvp_version])
AC_CONFIG_MACRO_DIR([m4])
m4_include([m4/ac_define_dir.m4])
m4_include([m4/ax_compare_version.m4])
m4_include([m4/basic-directories.m4])
AM_INIT_AUTOMAKE([1.11 gnu dist-xz dist-bzip2 subdir-objects])
AC_LANG([C++])
AC_CONFIG_HEADERS([config.h])

# ===========================
# Find required base packages
# ===========================
AC_PROG_CC
AC_PROG_CXX
AX_CXX_COMPILE_STDCXX([11],[noext],[mandatory])
AM_PROG_CC_C_O
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_LIBTOOL
PKG_PROG_PKG_CONFIG([0.20])

# =================
# Check for Poppler
# =================
AC_ARG_ENABLE(poppler, AS_HELP_STRING([--enable-poppler],[enable Poppler-based filters]),
	      enable_poppler=$enableval,enable_poppler=yes)
AM_CONDITIONAL(ENABLE_POPPLER, test x$enable_poppler = xyes)
if test x$enable_poppler = xyes; then
	PKG_CHECK_MODULES([POPPLER], [poppler >= 0.18], [
		PKG_CHECK_MODULES([POPPLER_CPP], [poppler-cpp >= 0.18], [
			AC_CHECK_HEADER([poppler/cpp/poppler-version.h],[AC_DEFINE([HAVE_CPP_POPPLER_VERSION_H],[],[Define if you have Poppler's "cpp/poppler-version.h" header file.])
			],[AC_MSG_ERROR([Could not identify the version of poppler-cpp])])
			],[AC_MSG_ERROR([poppler-cpp's "cpp/poppler-version.h" is needed.])
		])
	], [])
fi

# ================================
# Find CUPS internals (no pc file)
# ================================
AC_ARG_WITH([cups-config],
	[AS_HELP_STRING([--with-cups-config=path], [Specify path to cups-config executable.])],
	[with_cups_config="$withval"],
	[with_cups_config=system]
)

AS_IF([test "x$with_cups_config" != "xsystem"], [
	CUPSCONFIG=$with_cups_config
], [
	AC_PATH_TOOL(CUPSCONFIG, [cups-config])
	AS_IF([test -z "$CUPSCONFIG"], [
		AC_MSG_ERROR([Required cups-config is missing. Please install CUPS developer packages.])
	])
])
AC_SUBST(CUPS_VERSION)
CUPS_VERSION=`$CUPSCONFIG --version`

CUPS_CFLAGS=`$CUPSCONFIG --cflags`
AC_SUBST(CUPS_CFLAGS)

CUPS_LIBS=`$CUPSCONFIG --image --libs`
AC_SUBST(CUPS_LIBS)

CUPS_SERVERBIN=`$CUPSCONFIG --serverbin`
AC_SUBST(CUPS_SERVERBIN)

CUPS_DATADIR="`$CUPSCONFIG --datadir`"
AC_SUBST(CUPS_DATADIR)
AC_DEFINE_UNQUOTED(CUPS_DATADIR, "$CUPS_DATADIR", [CUPS datadir])

# ================================
# Find Dynamic Loader
# ================================
AC_SEARCH_LIBS([dlopen],
	[dl],
	[AS_IF([test "$ac_cv_search_dlopen" != "none required"], [
		DLOPEN_LIBS="$ac_cv_search_dlopen"
	])],
	AC_MSG_ERROR([unable to find the dlopen() function])
)
AC_SUBST(DLOPEN_LIBS)

# ================================
# Find Image Libraries
# ================================
AC_ARG_WITH([jpeg],
	[AS_HELP_STRING([--without-jpeg], [Disable jpeg support.])],
	[with_jpeg="$withval"],
	[with_jpeg=yes]
)
AS_IF([test x"$with_jpeg" != "xno"], [
	AC_DEFINE([HAVE_LIBJPEG], [], [Defines if we provide jpeg library.])
	AC_CHECK_HEADERS([jpeglib.h])
	AC_SEARCH_LIBS([jpeg_destroy_decompress],
		[jpeg],
		LIBJPEG_LIBS="-ljpeg",
		AC_MSG_ERROR([jpeg libraries not found.])
	)
	AC_SUBST(LIBJPEG_LIBS)
])

AC_ARG_WITH([png],
	[AS_HELP_STRING([--without-png], [Disable png support.])],
	[with_png="$withval"],
	[with_png=yes]
)
AS_IF([test x"$with_png" != "xno"], [
	PKG_CHECK_MODULES([LIBPNG], [libpng])
	AC_DEFINE([HAVE_LIBPNG], [], [Defines if we provide png library.])
])

AC_ARG_WITH([tiff],
	[AS_HELP_STRING([--without-tiff], [Disable tiff support.])],
	[with_tiff="$withval"],
	[with_tiff=yes]
)
AS_IF([test x"$with_tiff" != "xno"], [
	AC_DEFINE([HAVE_LIBTIFF], [], [Defines if we provide tiff library.])
	AC_CHECK_HEADERS([tiff.h])
	AC_SEARCH_LIBS([TIFFReadScanline],
		[tiff],
		LIBJPEG_LIBS="-ltiff",
		AC_MSG_ERROR([tiff libraries not found.])
	)
	AC_SUBST(LIBTIFF_LIBS)
])

# ----------------------------------------------
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
