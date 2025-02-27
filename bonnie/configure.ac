#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_INIT([bonnie++], [1.03e])
AC_CONFIG_SRCDIR([bonnie++.cpp])
# Add Automake
AM_INIT_AUTOMAKE([foreign])
AM_CONFIG_HEADER([config.h])

# Variables
AC_SUBST(stripping)
AC_ARG_ENABLE(stripping,
      [  --disable-stripping  disables stripping of installed binaries],
      STRIPPING=$strippingval, STRIPPING=no)
if [[ ! "$STRIPPING" = "no" ]]; then
  stripping=""
else
  stripping="-s"
fi
echo $DEB_BUILD_OPTIONS | grep -q nostrip
if [[ "$?" = "0" ]]; then
  stripping=""
fi

AC_SUBST(debug)
AC_ARG_ENABLE(debug,
      [  --with-debug  enables debug code generation for binaries],
      debug=-g, debug="")
echo $DEB_BUILD_OPTIONS | grep -q debug
if [[ "$?" = "0" ]]; then
  debug=-g
fi

# Checks for programs.
AC_LANG_CPLUSPLUS
AC_PROG_CXX
AC_PROG_CXXCPP
AC_PROG_CC
AC_PROG_INSTALL

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_SIZE_T

AC_SUBST(semun)
AC_TRY_COMPILE(#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>
    , union semun sem , semun="yes")
if [[ -n "$semun" ]]; then
   semun="#define SEMUN_IN_SEM_H"
fi
AC_SUBST(sa_sigaction)
AC_TRY_COMPILE([#include <signal.h>]
             , [struct sigaction sa; sa.sa_sigaction = 0]
             , sa_sigaction=y)
if [[ "$sa_sigaction" = "y" ]]; then
  sa_sigaction="#define USE_SA_SIGACTION"
fi
AC_SUBST(bool)
AC_TRY_COMPILE([], [bool var;] , , bool="typedef bool char;")
AC_SUBST(true_false)
AC_TRY_COMPILE(, [char c = true; char d = false;
] , true_false="0", true_false="1")
AC_SUBST(snprintf)
AC_TRY_LINK([#include <stdio.h>], char buf[[10]]; snprintf(buf, sizeof(buf), "abc");,,snprintf="no");
if [[ -n "$snprintf" ]]; then
   snprintf="#define NO_SNPRINTF"
fi

AC_SUBST(exes)
AC_SUBST(man8)
# Checks for header files.
AC_CHECK_HEADERS([fcntl.h stdlib.h string.h sys/time.h unistd.h])
AC_CHECK_HEADERS(vector.h vector, vec_exes="zcav" vec_man8="zcav.8", echo "STL not installed so no ZCAV!")
exes="bonnie++ $vec_exes"
man8="bonnie++.8 $vec_man8"
AC_CHECK_HEADERS(algorithm algo.h algo)

AC_SUBST(aix_mem_size)
if [[ -f /etc/objrepos ]]; then
  aix_mem_size="#define AIX_MEM_SIZE 1"
fi

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_TYPE_UID_T
AC_TYPE_OFF_T
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T

# Checks for library functions.
AC_FUNC_FORK
AC_CHECK_FUNCS([getpagesize gettimeofday memset rmdir strdup uname])

# Pass Simulator
AC_SUBST([SIMULATOR])
AC_CONFIG_FILES([Makefile])
AC_OUTPUT([bonnie.h port.h bonnie++.spec bon_csv2html bon_csv2txt sun/pkginfo bonnie-run.sh],[chmod a+x bonnie-run.sh])
