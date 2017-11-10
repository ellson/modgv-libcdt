#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

#########################################################################
# Copyright (c) 2017 AT&T Intellectual Property
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
#
# Contributors: John Ellson <john.ellson@gmail.com>
#########################################################################

AC_PREREQ([2.69])

# set:
#   version_major
#   version_minor
#   version_micro
#   collection
#   version_date
#   version_commit
m4_include(./version.m4)

AC_INIT([libcdt], [version_major().version_minor().version_micro()], [john.ellson@gmail.com])

LIBCDT_VERSION_MAJOR=version_major()
LIBCDT_VERSION_MINOR=version_minor()
LIBCDT_VERSION_MICRO=version_micro()

#NB: "stable" or "development"
LIBCDT_COLLECTION=collection()

#NB: date/time of last commit - or "0"
LIBCDT_VERSION_DATE=version_date()

#NB: git commit hash of last commit
LIBCDT_VERSION_COMMIT=version_commit()

LIBCDT_SOURCES=g/$LIBCDT_COLLECTION/SOURCES

AC_SUBST([LIBCDT_VERSION_MAJOR])
AC_SUBST([LIBCDT_VERSION_MINOR])
AC_SUBST([LIBCDT_VERSION_MICRO])
AC_SUBST([LIBCDT_VERSION_DATE])
AC_SUBST([LIBCDT_VERSION_COMMIT])
AC_SUBST([LIBCDT_COLLECTION])
AC_SUBST([LIBCDT_SOURCES])

dnl ===========================================================================

AC_CONFIG_SRCDIR([configure.ac])
AC_CONFIG_AUX_DIR([config])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CC
AC_PROG_LN_S
AC_PROG_MAKE_SET

# Checks for header files.
AC_CHECK_HEADERS([ \
    stddef.h \
    stdlib.h \
    string.h \
])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_MODE_T
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UID_T

AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE([no-define])
# Suppress verbose compile lines
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_PROG_LIBTOOL
AC_CHECK_PROG(GROFF,groff,groff,false)
AC_CHECK_PROG(PS2PDF,ps2pdf,ps2pdf,false)
AC_CHECK_PROG(PS2PDF,pstopdf,pstopdf,false)

# check for pkgconfig
PKG_PROG_PKG_CONFIG

AC_PREFIX_DEFAULT([/usr/local])
if test "x${prefix}" = "xNONE"; then
    prefix=${ac_default_prefix}
    AC_SUBST([prefix])
fi

# automatically check for other dependencies in $prefix first
    if test "x${prefix}" != "x/usr"; then
        CPPFLAGS="$CPPFLAGS -I${prefix}/include"
        LDFLAGS="$LDFLAGS -L${prefix}/lib"
        PKG_CONFIG_PATH="$prefix/lib/pkgconfig$PATHSEPARATOR$PKG_CONFIG_PATH"
        export PKG_CONFIG_PATH
fi

# debug
AC_ARG_ENABLE([debug],
    AS_HELP_STRING([--enable-debug],[Compile the debug version (default: disabled)]),
        [enable_debug=$enableval],
        [enable_debug=no])
AM_CONDITIONAL([DEBUG], [test $enable_debug = "yes"])
if test "x$enable_debug" = "xyes"; then
    changequote({,})
    CFLAGS=`echo "$CFLAGS" | $SED -e 's/-O[0-9s]*//g'`
    CXXFLAGS=`echo "$CXXFLAGS" | $SED -e 's/-O[0-9s]*//g'`
    CFLAGS=`echo "$CFLAGS" | $SED -e 's/-g//g'`
    CXXFLAGS=`echo "$CXXFLAGS" | $SED -e 's/-g//g'`
    changequote([,])
    # dnl add -O0 only if GCC or ICC is used
    if test "$GCC" = "yes" || test "$ICC" = "yes"; then
        CFLAGS="${CFLAGS} -g -O0 -Wall"
        CXXFLAGS="${CXXFLAGS} -g -O0 -Wall"

	# Enable common extra flags
        CFLAGS="${CFLAGS} -Wextra"

        # Enable specific warning flags not included by -Wall or -Wextra
        CFLAGS="${CFLAGS} -Wmissing-include-dirs -Wfloat-equal -Wundef -Wshadow -Wpointer-arith -Wbad-function-cast -Wcast-qual -Wconversion -Wstrict-prototypes -Wmissing-prototypes -Wnested-externs"

        # Not this as it prevent compiler detecting missing enum cases: -Wswitch-default

        # and more from: https://kristerw.blogspot.com/2017/09/useful-gcc-warning-options-not-enabled.html
        CXXFLAGS="${CXXFLAGS} -Wold-style-cast -Wuseless-cast"
        CFLAGS="${CFLAGS} -Wduplicated-cond -Wduplicated-branches -Wlogical-op -Wrestrict -Wnull-dereference -Wjump-misses-init -Wdouble-promotion -Wshadow -Wformat=2 "

        # set flag for conditional debugging code
        CFLAGS="${CFLAGS} -DDEBUG"
    fi
fi

AC_CONFIG_FILES([Makefile \
                 libcdt.spec \
                 src/Makefile \
                 src/libcdt.pc \
                 Doxyfile \
])
AC_OUTPUT
