AC_INIT([XMPPNotifier], [0.1]) dnl package name, version
                    
dnl Now find the compiler and compiler flags to use
: ${R_HOME=`R RHOME`}
if test -z "${R_HOME}"; then
 echo "could not determine R_HOME"
 exit 1
fi
CC=`"${R_HOME}/bin/R" CMD config CC`
CPP=`"${R_HOME}/bin/R" CMD config CPP`
CFLAGS=`"${R_HOME}/bin/R" CMD config CFLAGS`
CPPFLAGS=`"${R_HOME}/bin/R" CMD config CPPFLAGS`

AC_PROG_CC
AC_PROG_CPP

# Check for pkg-config program, used for configuring some libraries.
#
m4_define_default([PKG_PROG_PKG_CONFIG],
[AC_MSG_CHECKING([pkg-config])
AC_MSG_RESULT([no])])

PKG_PROG_PKG_CONFIG

# If the pkg-config autoconf support isn't installed, define its
# autoconf macro to disable any packages depending on it.
#
m4_define_default([PKG_CHECK_MODULES],
[AC_MSG_CHECKING([$1])
AC_MSG_RESULT([no])
$4])

AC_CHECK_HEADER(/usr/include/openssl/ssl.h, [], [AC_MSG_ERROR([could not find openssl headers, openssl required])])
AC_CHECK_HEADER(resolv.h, [LIBS="$LIBS -lresolv"], [AC_MSG_ERROR([could not find resolv headers, resolv required])])

PKG_CHECK_MODULES(libxml2, [libxml-2.0 >= 2.7], 
                  [HAVE_LIBXML2="yes"], [])
if test "$HAVE_LIBXML2" = "yes"; then
  LIBS="$LIBS $libxml2_LIBS"
  CFLAGS=$libxml2_CFLAGS
else
#  AC_MSG_NOTICE([Test libxml without pkg-config])
  XTRAINCPATHS="-I/usr/include/libxml2/"
  CFLAGS="$CFLAGS $XTRAINCPATHS"
  CPPFLAGS="$CPPFLAGS $XTRAINCPATHS"
  AC_CHECK_HEADER([libxml2/libxml/parser.h],
                  [HAVE_LIBXML2="yes"],
                  [AC_MSG_ERROR([could not find libxml2, libxml2 required])])
  LIBS="$LIBS -lxml2"
  CFLAGS="-I /usr/include/libxml2"
fi

dnl and do substitution in the src/Makevars.in and src/config.h
AC_CONFIG_FILES([src/Makevars])
AC_OUTPUT()
