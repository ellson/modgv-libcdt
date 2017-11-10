# $Id$ $Revision$
## Process this file with automake to produce Makefile.in

CDT_VERSION="5:0:0"

pkgconfigdir = $(libdir)/pkgconfig

AM_CPPFLAGS = -I$(top_srcdir)

include_HEADERS = cdt.h
noinst_HEADERS = dthdr.h
noinst_LTLIBRARIES = libcdt_C.la
lib_LTLIBRARIES = libcdt.la
man_MANS = cdt.3
pkgconfig_DATA = libcdt.pc

libcdt_C_la_SOURCES = dtclose.c dtdisc.c dtextract.c dtflatten.c \
	dthash.c dtlist.c dtmethod.c dtopen.c dtrenew.c dtrestore.c dtsize.c \
	dtstat.c dtstrhash.c dttree.c dtview.c dtwalk.c

libcdt_la_LDFLAGS = -version-info $(CDT_VERSION) -no-undefined
libcdt_la_SOURCES = $(libcdt_C_la_SOURCES)

EXTRA_DIST = $(man_MANS) cdt.vcxproj*
