AC_INIT([libstrophe], [trunk], [jack@metajack.im])
AM_INIT_AUTOMAKE

LT_INIT
AC_PROG_CC
AM_PROG_CC_C_O

AC_CHECK_HEADER(openssl/ssl.h, [], [AC_MSG_ERROR([couldn't find openssl headers, openssl required])])

AC_ARG_WITH([libxml2], 
            [AS_HELP_STRING([--with-libxml2], [use libxml2 for XML parsing])],
            [with_libxml2=yes],
            [with_libxml2=no])

if test "x$with_libxml2" != xno; then
  PKG_CHECK_MODULES([libxml2], [libxml-2.0 >= 2.7],
                    [with_libxml2=yes], 
                    [AC_MSG_ERROR([couldn't find libxml2])])
else
  AC_CHECK_FUNCS(memmove)
  AC_C_BIGENDIAN([byteorder=1234], [byteorder=4321], [], [])
fi

if test "x$with_libxml2" = xyes; then
  with_parser=libxml2
  PARSER_NAME=libxml2
  PARSER_CFLAGS=\$\(libxml2_CFLAGS\)
  PARSER_LIBS=\$\(libxml2_LIBS\)
  AC_CHECK_LIB(xml2, xmlFree, [], [AC_MSG_ERROR("libxml2 not found")])
else
  with_parser=builtin
  PARSER_NAME=expat
  PARSER_CFLAGS=\$\(builtin_CFLAGS\)
  PARSER_LIBS=\$\(builtin_LIBS\)
fi

AC_CHECK_LIB(ssl, ssl_verify_cert_chain, [], [AC_MSG_ERROR("libssl not found")])

# Check for the resolv library (needed on linux systems)
AC_CHECK_LIB(resolv, __res_query)

AC_MSG_NOTICE([libstrophe will use the $with_parser XML parser])
AC_SEARCH_LIBS([socket], [socket])

AM_CONDITIONAL([BUILD_EXPAT], [test x$with_parser = xbuiltin])
AM_CONDITIONAL([PARSER_EXPAT], [test x$with_parser != xlibxml2])
AC_DEFINE_UNQUOTED([BYTEORDER], [$byteorder])
AC_SUBST(PARSER_NAME)                              
AC_SUBST(PARSER_CFLAGS)
AC_SUBST(PARSER_LIBS)
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
