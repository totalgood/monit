# Process this file with bootstrap to produce a configure script.

# autoconf requirement
AC_PREREQ([2.53])

# Note: in  case of beta subversion, use underscore "_" rather then dash "-"
# since RPM doesn't allow dash in Version
# Example: 5.0_beta2
AC_INIT([monit], [5.19], [monit-general@nongnu.org])
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE
AC_CONFIG_SRCDIR([src/monit.c])
AC_CONFIG_SUBDIRS([libmonit])
AC_CONFIG_COMMANDS([libtool_patch],[test `uname` = "OpenBSD" && perl -p -i -e "s/deplibs_check_method=.*/deplibs_check_method=pass_all/g" libtool])
AC_CONFIG_COMMANDS([monitrc], [chmod 600 monitrc])

# ------------------------------------------------------------------------
# Programs 
# ------------------------------------------------------------------------

AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_MAKE_SET
AC_CHECK_PROGS([YACC], ['bison -y' byacc yacc], [no], [$PATH:/usr/local/bin:/usr/bin])
if test "x$YACC" = "xno"; then
	# Require bison unless y.tab.c already is built
	if test ! -f src/y.tab.c; then
                AC_MSG_ERROR([Monit require bison, byacc or yacc. Download bison from http://www.gnu.org/software/bison/])
	fi
fi
AC_PATH_PROG([FLEX], [flex], [no], [$PATH:/usr/local/bin:/usr/bin])
if test "x$FLEX" = "xno"; then
	# Require flex unless lex.yy.c already is built
	if test ! -f src/lex.yy.c; then
                AC_MSG_ERROR([flex is required. Download from http://www.gnu.org/software/flex/])
	fi
fi
AC_PATH_PROG([POD2MAN], [pod2man], [no], [$PATH:/usr/local/bin:/usr/bin])
if test "x$POD2MAN" = "xno"; then
	# Require pod2man unless monit.1 already is built
	if test ! -f monit.1; then
		AC_MSG_ERROR([pod2man is required to build the monit.1 man file.])
	fi
else
	POD2MANFLAGS="--center 'User Commands' --release AC_PACKAGE_VERSION --date='www.mmonit.com' --lax"
	AC_SUBST([POD2MANFLAGS])
fi


# ------------------------------------------------------------------------
# Libtool 
# ------------------------------------------------------------------------

AC_DISABLE_STATIC
AC_PROG_LIBTOOL


# ------------------------------------------------------------------------
# Libraries 
# ------------------------------------------------------------------------

# Check for libraries
AC_CHECK_LIB([socket], [socket])
AC_CHECK_LIB([inet],   [socket])
AC_CHECK_LIB([nsl],    [inet_addr])
AC_CHECK_LIB([resolv], [inet_aton])
AC_CHECK_LIB([c], [crypt], [:], [AC_CHECK_LIB([crypt], [crypt])])
AC_CHECK_LIB([pthread], [pthread_create], [], [AC_MSG_ERROR([POSIX thread library is required])])


# ------------------------------------------------------------------------
# Header files 
# ------------------------------------------------------------------------

AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_HEADER_STAT
AC_HEADER_TIME

AC_CHECK_HEADERS([ \
        alloca.h \
	arpa/inet.h \
	asm/page.h \
	asm/param.h \
	cf.h \
	crt_externs.h \
	ctype.h \
	crypt.h \
	dirent.h \
	errno.h \
	execinfo.h \
	fcntl.h \
	getopt.h \
	glob.h \
	grp.h \
	ifaddrs.h \
	kinfo.h \
	kvm.h \
	paths.h \
	kstat.h \
	zone.h \
	sys/protosw.h \
	libproc.h \
	limits.h \
	loadavg.h \
	locale.h \
        mach/boolean.h \
        mach/host_info.h \
        mach/mach.h \
        mach/mach_host.h \
	memory.h \
	mntent.h \
	netdb.h \
        sys/socket.h \
	netinet/in.h \
	netinet/tcp.h \
        netinet/in_systm.h \
        pam/pam_appl.h \
        security/pam_appl.h \
	poll.h \
	procfs.h \
	sys/procfs.h \
	procinfo.h \
	pthread.h \
	pwd.h \
	regex.h \
	setjmp.h \
	signal.h \
	stdarg.h \
        stddef.h \
	stdio.h \
	string.h \
	strings.h \
	stropts.h \
	sys/cfgodm.h \
	sys/cfgdb.h \
	sys/dk.h \
	sys/dkstat.h \
	sys/filio.h \
	sys/ioctl.h \
	sys/loadavg.h \
	sys/lock.h \
	sys/mnttab.h \
	sys/mutex.h \
	sys/nlist.h \
	sys/param.h \
	sys/pstat.h \
	sys/queue.h \
	sys/resource.h \
	sys/sched.h \
	sys/statfs.h \
	sys/statvfs.h \
	sys/sysinfo.h \
	sys/systemcfg.h \
	sys/time.h \
	sys/tree.h \
	sys/types.h \
	sys/un.h \
	sys/utsname.h \
        sys/vmmeter.h \
        sys/vm_usage.h \
	sys/vfs.h \
	syslog.h \
	unistd.h \
	utmpx.h \
        uvm/uvm_extern.h \
        uvm/uvm_param.h \
        vm/vm.h \
	])

AC_CHECK_HEADERS([ \
	libperfstat.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_PROTOSW_H
         #include <sys/protosw.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	netinet/ip.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_TYPES_H
         #include <sys/types.h>
         #endif
         #if HAVE_NETINET_IN_SYSTM_H
         #include <netinet/in_systm.h>
         #endif
         #if HAVE_NETINET_IN_SYSTM_H
         #include <netinet/in.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	net/if.h \
	netinet/ip_icmp.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_TYPES_H
         #include <sys/types.h>
         #endif
         #if HAVE_SYS_SOCKET_H
         #include <sys/socket.h>
         #endif
         #if HAVE_NETINET_IN_SYSTM_H
         #include <netinet/in.h>
         #endif
         #if HAVE_NETINET_IN_SYSTM_H
         #include <netinet/in_systm.h>
         #endif
         #if HAVE_NETINET_IP_H
         #include <netinet/ip.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	netinet/icmp6.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_TYPES_H
         #include <sys/types.h>
         #endif
         #if HAVE_NETINET_IP_H
         #include <netinet/in.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
        sys/sysctl.h \
        sys/mount.h \
        sys/proc.h \
        sys/swap.h \
	sys/ucred.h \
        sys/user.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_PARAM_H
         #include <sys/param.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	machine/vmparam.h \
        vm/pmap.h \
        machine/pmap.h \
        vm/vm_map.h \
	vm/vm_object.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_PARAM_H
         #include <sys/param.h>
         #endif
         #ifdef HAVE_SYS_QUEUE_H
         #include <sys/queue.h>
         #endif
         #ifdef HAVE_SYS_LOCK_H
         #include <sys/lock.h>
         #endif
         #ifdef HAVE_SYS_MUTEX_H
         #include <sys/mutex.h>
         #endif
         #ifdef HAVE_VM_VM_H
         #include <vm/vm.h>
         #endif
         #ifdef HAVE_VM_PMAP_H
         #include <vm/pmap.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
        sys/resourcevar.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_TIME_H
         #include <sys/time.h>
         #endif
         #ifdef HAVE_SYS_RESOURCE_H
         #include <sys/resource.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	uvm/uvm_map.h \
	uvm/uvm_pmap.h \
	uvm/uvm_object.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_TIME_H
         #include <sys/time.h>
         #endif
         #ifdef HAVE_SYS_LOCK_H
         #include <sys/lock.h>
         #endif
         #ifdef HAVE_SYS_TREE_H
         #include <sys/tree.h>
         #endif
         #ifdef HAVE_UVM_UVM_EXTERN_H
         #include <uvm/uvm_extern.h>
         #endif
        ])

AC_CHECK_HEADERS([ \
	uvm/uvm.h \
        ],
        [],
        [],
        [
         #ifdef HAVE_SYS_MUTEX_H
         #include <sys/mutex.h>
         #endif
         #ifdef HAVE_SYS_TYPES_H
         #include <sys/types.h>
         #endif
         #ifdef HAVE_SYS_PARAM_H
         #include <sys/param.h>
         #endif
         #ifdef HAVE_UVM_UVM_PARAM_H
         #include <uvm/uvm_param.h>
         #endif
        ])

# ------------------------------------------------------------------------
# Types
# ------------------------------------------------------------------------

AC_CHECK_TYPES([boolean_t], [], [],
        [
         #ifdef HAVE_SYS_TYPES_H
         #include <sys/types.h>
         #endif
         #ifdef HAVE_MACH_BOOLEAN_H
         #include <mach/boolean.h>
         #endif
         #ifdef HAVE_UVM_UVM_PARAM_H
         #include <uvm/uvm_param.h>
         #endif
         #ifdef HAVE_VM_VM_H
         #include <vm/vm.h>
         #endif
         #ifdef HAVE_KINFO_H
         #include <kinfo.h>
         #endif
        ])

AC_TYPE_MODE_T
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_TYPE_PID_T
AC_TYPE_SIGNAL

# Check for structures.
AC_STRUCT_TM
AC_CHECK_MEMBERS([struct tm.tm_gmtoff])


# ------------------------------------------------------------------------
# Functions 
# ------------------------------------------------------------------------

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_FUNC_FORK
AC_FUNC_STAT
AC_FUNC_STRFTIME
AC_CHECK_FUNCS(statfs)
AC_CHECK_FUNCS(statvfs)
AC_CHECK_FUNCS(setlocale)
AC_CHECK_FUNCS(getaddrinfo)
AC_CHECK_FUNCS(syslog)
AC_CHECK_FUNCS(vsyslog)
AC_CHECK_FUNCS(backtrace)
AC_CHECK_FUNCS(getloadavg)
AC_CHECK_FUNCS(getopt_long)

AC_MSG_CHECKING(for va_copy)
AC_TRY_LINK([
	#include <stdarg.h>
], [
	va_list ap;
	va_list ap_copy;
	va_copy(ap, ap_copy);
], [
	AC_MSG_RESULT(yes)
	AC_DEFINE([HAVE_VA_COPY], [1], [Define to 1 if VA_COPY is defined.])
], [
	AC_MSG_RESULT(no)
])


# ------------------------------------------------------------------------
# Compiler 
# ------------------------------------------------------------------------

# Compiler characteristics
AC_C_CONST
AC_C_BIGENDIAN

# If the compiler is gcc, tune warnings and make the char type unsigned 
# and enable C99 support
if test "x$GCC" = "xyes"; then
	CFLAGS="$CFLAGS -Wall -Wunused -Wno-unused-label -funsigned-char";
        # Add C99 support
        CFLAGS="$CFLAGS -D_GNU_SOURCE -std=c99"
        # does this compiler support -Wno-pointer-sign ?
        svd_CFLAGS="$CFLAGS"
        CFLAGS="-Wno-pointer-sign $CFLAGS"
        AC_COMPILE_IFELSE([AC_LANG_PROGRAM([], [return 0;])], [], [CFLAGS="$svd_CFLAGS"])        
        # does this compiler support -Wno-address ?
        svd_CFLAGS="$CFLAGS"
        CFLAGS="-Wno-address $CFLAGS"
        AC_COMPILE_IFELSE([AC_LANG_PROGRAM([], [return 0;])], [], [CFLAGS="$svd_CFLAGS"])
fi


# ------------------------------------------------------------------------
# IPv6 Support
# ------------------------------------------------------------------------

AC_MSG_CHECKING(for IPv6 support)
AC_CACHE_VAL(ac_cv_ipv6,
        AC_TRY_RUN([
        #include <sys/types.h>
        #include <sys/socket.h>
        #include <netinet/in.h>
        /* Make sure the definitions for AF_INET6 and struct sockaddr_in6
         * are defined, and that we can actually create an IPv6 TCP socket.*/
        main()
        {
                int fd;
                struct sockaddr_in6 foo;
                fd = socket(AF_INET6, SOCK_STREAM, 0);
                exit(fd >= 0 ? 0 : 1);
        }],
        ac_cv_ipv6=yes,
        ac_cv_ipv6=no,
        ac_cv_ipv6=no))
AC_MSG_RESULT($ac_cv_ipv6)
if test $ac_cv_ipv6 = yes ; then
        AC_DEFINE([HAVE_IPV6], 1, [Define to 1 if the system supports IPv6])
fi


# ------------------------------------------------------------------------
# Paths 
# ------------------------------------------------------------------------

# Find the right directory to put the root-mode PID file in
AC_MSG_CHECKING([pid file location])
if test -d "/run"
then
	piddir="/run"
elif test -d "/var/run"; then
        piddir="/var/run"
elif test -d "/etc"; then
        piddir="/etc"
fi

AC_DEFINE_UNQUOTED([PIDDIR], "$piddir",
	  [Define to the pid storage directory.])
AC_MSG_RESULT([$piddir])

# Test mounted filesystem description file
if test -f "/etc/mtab"
then
  AC_DEFINE([HAVE_MTAB], 1, [Define to 1 if you have /etc/mtab])
elif test -f "/etc/mnttab"; then
  AC_DEFINE([HAVE_MNTTAB], 1, [Define to 1 if you have /etc/mnttab])
fi


# ------------------------------------------------------------------------
# Architecture/OS detection
# ------------------------------------------------------------------------

# Backward compatibility until we get ride of arch settings
architecture=`uname`
if test "$architecture" = "SunOS" 
then
   ARCH="SOLARIS"
   CFLAGS="$CFLAGS -D _REENTRANT -D_POSIX_PTHREAD_SEMANTICS -D__EXTENSIONS__ -m64"
   LDFLAGS="$LDFLAGS -m64"
   test_kstat="true"
   AC_DEFINE([HAVE_CPU_WAIT], [1], [Define to 1 if CPU wait information is available.])
   if test `uname -m` = "i86pc"
   then
      if test "x$GCC" = "xyes"
      then
            CFLAGS="$CFLAGS -mtune=opteron"
            LDFLAGS="$LDFLAGS -mtune=opteron"
      else
            CFLAGS="$CFLAGS -xarch=sse2"
            LDFLAGS="$LDFLAGS -xarch=sse2"
      fi
   else
      if test "x$GCC" = "xyes"
      then
            CFLAGS="$CFLAGS -mtune=v9"
            LDFLAGS="$LDFLAGS -mtune=v9"
      else
            CFLAGS="$CFLAGS -xarch=sparc"
            LDFLAGS="$LDFLAGS -xarch=sparc"
      fi
   fi
elif test "$architecture" = "Linux" 
then
   ARCH="LINUX"
   CFLAGS="$CFLAGS -D _REENTRANT"
   LDFLAGS="$LDFLAGS -rdynamic"
   if test `uname -r | awk -F '.' '{print$1$2}'` -ge "26"
   then
   	AC_DEFINE([HAVE_CPU_WAIT], [1], [Define to 1 if CPU wait information is available.])
   fi
elif test "$architecture" = "HP-UX"
then
   ARCH="HPUX"
   CFLAGS="$CFLAGS -D _REENTRANT -D _INCLUDE_XOPEN_SOURCE -D _INCLUDE_XOPEN_SOURCE_EXTENDED -D _XOPEN_SOURCE_EXTENDED -D _INCLUDE_HPUX_SOURCE"
   AC_DEFINE([HAVE_CPU_WAIT], [1], [Define to 1 if CPU wait information is available.])
elif test "$architecture" = "OpenBSD"
then
   ARCH="OPENBSD"
   CFLAGS="$CFLAGS -D _REENTRANT"
   test_kvm="true"
   use_pam=0 # No PAM on OpenBSD (supports BSD Auth API instead of PAM)
elif test "$architecture" = "FreeBSD"
then
   ARCH="FREEBSD"
   CFLAGS="$CFLAGS -D _REENTRANT"
   test_kvm="true"
elif test "$architecture" = "GNU/kFreeBSD"
then
   ARCH="FREEBSD"
   CFLAGS="$CFLAGS -D _REENTRANT"
   test_kvm="true"
elif test "$architecture" = "NetBSD"
then
   ARCH="NETBSD"
   CFLAGS="$CFLAGS -D _REENTRANT"
   test_kvm="true"
elif test "$architecture" = "DragonFly"
then
   ARCH="DRAGONFLY"
   CFLAGS="$CFLAGS -D _REENTRANT"
   test_kvm="true"
elif test "$architecture" = "Darwin"
then
   ARCH="DARWIN"
   CFLAGS="$CFLAGS -DREENTRANT -no-cpp-precomp -DNEED_SOCKLEN_T_DEFINED"
   test_kvm="true"
   LIBS="$LIBS -framework System -multiply_defined suppress"
elif test "$architecture" = "AIX"
then
   ARCH="AIX"
   CFLAGS=`echo $CFLAGS|sed 's/-g//g'`
   CFLAGS="$CFLAGS -D_THREAD_SAFE -D_REENTRANT"
   LIBS="$LIBS -lodm -lperfstat"
   AC_DEFINE([HAVE_CPU_WAIT], [1], [Define to 1 if CPU wait information is available.])
else
   AC_MSG_WARN([Architecture not supported: ${architecture}])
   CFLAGS="$CFLAGS -D _REENTRANT"
   ARCH="UNKNOWN"
fi
AC_SUBST(ARCH)


# ------------------------------------------------------------------------
# Resource code
# ------------------------------------------------------------------------

# Test for kvm libs
kvm_libs=""
if test "$test_kvm"; then
       	AC_CHECK_LIB([kvm],
        	     [kvm_open],
        	     [kvm_libs="-lkvm"])
       	if test "$kvm_libs"
       	then
       	        LIBS="$LIBS $kvm_libs"
       	fi
fi
kstat_libs=""
if test "$test_kstat"; then
       	AC_CHECK_LIB([kstat],
        	     [kstat_open],
        	     [kstat_libs="-lkstat"])
       	if test "$kstat_libs"
       	then
       	        LIBS="$LIBS $kstat_libs"
       	fi
fi


# ------------------------------------------------------------------------
# Large files code
# ------------------------------------------------------------------------

# Check if we want to have large files support
AC_MSG_CHECKING([for large files support])
AC_ARG_WITH(largefiles,
    [  --without-largefiles    disable large files support (default: enabled)],
    [
        dnl Check the withvalue                
        if test "x$withval" = "xno" ; then
            use_largefiles=0
            AC_MSG_RESULT([disabled])
        fi
        if test "x$withval" = "xyes" ; then
            use_largefiles=1
            AC_MSG_RESULT([enabled])
        fi
    ],
    [
	if test `uname` = "AIX"
	then
	    use_largefiles=0
	    AC_MSG_RESULT([disabled])
	else
	    use_largefiles=1
	    AC_MSG_RESULT([enabled])
	fi
    ]
)

# Settings for largefiles support
if test "$use_largefiles" = 1; then
        AC_SYS_LARGEFILE
        AC_DEFINE([HAVE_LARGEFILES], 1, [Define to 1 if you have large files support])
fi


# ------------------------------------------------------------------------
# PAM Code
# ------------------------------------------------------------------------

AC_MSG_CHECKING([for PAM support])
AC_ARG_WITH(pam,
    [  --without-pam           disable the use of pam (default: enabled)],
    [
        dnl Check the withvalue                
        if test "x$withval" = "xno" ; then
            use_pam=0
            AC_MSG_RESULT([disabled])
        fi
        if test "x$withval" = "xyes" ; then
            use_pam=1
            AC_MSG_RESULT([enabled])
        fi
    ],
    [
        if test "$use_pam" = "0"; then
            AC_MSG_RESULT([disabled])
        else
            use_pam=1
            AC_MSG_RESULT([enabled])
        fi
    ]
)

if test "$use_pam" = "1"; then
        AC_CHECK_LIB([pam], [pam_start], [], [AC_MSG_ERROR([PAM enabled but headers or library not found, install the PAM development support or run configure --without-pam])])
fi


# ------------------------------------------------------------------------
# SSL Code
# ------------------------------------------------------------------------

# Check for ssl includes
checksslincldir() { :
    if test -f "$1/openssl/ssl.h"; then
        sslincldir="$1"
        return 0
    fi
    return 1
}

# Check for ssl libraries 
checkssllibdirdynamic() { :
    CRYPTOLIB=`ls -1 $1/libcrypto.so* $1/libcrypto.dylib* 2>/dev/null | wc -l`
    SSLLIB=`ls -1 $1/libssl.so* $1/libssl.dylib* 2>/dev/null | wc -l`
    if test "(" $CRYPTOLIB -gt 0 -a $SSLLIB -gt 0 ")"
    then
        ssllibdir="$1"
        return 0
    fi
    return 1
}

checkssllibdirstatic() { :
    if test "(" -f "$1/libcrypto.a" ")"  -a \
            "(" -f "$1/libssl.a" ")" ; then
        ssllibdir="$1"
        return 0
    fi
    return 1
}

# Check if we want to have SSL

AC_MSG_CHECKING([for static SSL support])

AC_ARG_WITH(ssl-static,
    [  --with-ssl-static=DIR       location of SSL installation],
    [
        dnl Check the specified location only
        for dir in "$withval" "$withval/include"; do
            checksslincldir "$dir"
        done
        for dir in "$withval" "$withval/lib"; do
            checkssllibdirstatic "$dir" && break
        done
        use_sslstatic=1
        LDFLAGS="`echo $LDFLAGS | sed -e 's/-rdynamic/-ldl/g'`"
        AC_MSG_RESULT([enabled])
        AC_DEFINE([HAVE_OPENSSL], 1, [Define to 1 if you have openssl.])
        AC_SUBST(sslincldir)
        AC_SUBST(ssllibdir)
        CFLAGS="$CFLAGS -I$sslincldir"
        LIBS="$LIBS $ssllibdir/libssl.a $ssllibdir/libcrypto.a"
    ],
    [
	use_sslstatic=0
        AC_MSG_RESULT([disabled])
    ]
)

if test "$use_sslstatic" = "0"
then
    AC_MSG_CHECKING([for SSL support])

    AC_ARG_WITH(ssl,
        [  --without-ssl           disable the use of ssl (default: enabled)],
        [
            dnl Check the withvalue                
            if test "x$withval" = "xno" ; then
                use_ssl=0
                AC_MSG_RESULT([disabled])
            fi
            if test "x$withval" = "xyes" ; then
                use_ssl=1
                AC_MSG_RESULT([enabled])
            fi
        ],
        [
            use_ssl=1
            AC_MSG_RESULT([enabled])
        ]
    )


    # Check for SSL directory
    if test "$use_ssl" = "1"; then

        AC_ARG_WITH(ssl-dir,
            [  --with-ssl-dir=DIR       location of SSL installation],
            [
                dnl Check the specified location only
                for dir in "$withval" "$withval/include"; do
                	checksslincldir "$dir"
                done
                for dir in "$withval" "$withval/lib"; do
                    checkssllibdirdynamic "$dir" && break
                done
            ]
        )

        AC_MSG_CHECKING([for SSL include directory])
        AC_ARG_WITH(ssl-incl-dir,
            [  --with-ssl-incl-dir=DIR  location of installed SSL include files],
            [
                dnl Check the specified location only
                checksslincldir "$withval"
            ],
            [
                if test -z "$sslincldir"; then	
                    dnl Search default locations of SSL includes
                    for maindir in /usr /usr/local /usr/lib /usr/pkg /var /opt /usr/sfw /opt/csw /opt/freeware; do
                        for dir in "$maindir/include"\
                                   "$maindir/include/openssl"\ 
                                   "$maindir/include/ssl"\
                                   "$maindir/ssl/include"; do
                            checksslincldir $dir && break 2
                        done
                    done
                fi
            ]
        )
        if test -z "$sslincldir"; then
            AC_MSG_RESULT([Not found])
            echo
            echo "Couldn't find your SSL header files."
            echo "Use --with-ssl-incl-dir option to fix this problem or disable"
            echo "the SSL support with --without-ssl"
            echo
            exit 1
        fi
        AC_MSG_RESULT([$sslincldir])

        AC_MSG_CHECKING([for SSL library directory])
        AC_ARG_WITH(ssl-lib-dir,
            [  --with-ssl-lib-dir=DIR   location of installed SSL library files],
            [
                dnl Check the specified location only
                checkssllibdirdynamic "$withval"
            ],
            [
                if test -z "$ssllibdir"; then	
                    dnl Search default locations of SSL libraries
                    for maindir in "" \
                                   /usr \
                                   /usr/local \
                                   /usr/pkg \
                                   /var \
                                   /opt \
                                   /usr/sfw \
                                   /opt/csw \
                                   /opt/freeware; do
                        for dir in $maindir \
                                   $maindir/openssl \
                                   $maindir/ssl \
                                   $maindir/lib \
                                   $maindir/lib/openssl \
                                   $maindir/lib/ssl \
                                   $maindir/ssl/lib \
                                   $maindir/lib/64 \
                                   $maindir/lib/64/openssl \
                                   $maindir/lib/64/ssl \
                                   $maindir/ssl/lib/64 \
                                   $maindir/lib64 \
                                   $maindir/lib64/openssl \
                                   $maindir/lib64/ssl \
                                   $maindir/ssl/lib64 \
                                   $maindir/lib/${build} \
                                   $maindir/lib/${build_alias}; do
                            checkssllibdirdynamic $dir && break 2
                        done
                    done
                fi
            ]
        )
        if test -z "$ssllibdir"; then
            AC_MSG_RESULT([Not found])
            dnl Let the compiler find the library using default paths
            AC_CHECK_LIB([ssl], [SSL_library_init], [], [AC_MSG_ERROR([Could not find SSL library, please use --with-ssl-lib-dir option or disabled the SSL support using --without-ssl])])
            AC_CHECK_LIB([crypto],[CRYPTO_new_ex_data], [], [AC_MSG_ERROR([Could not find SSL library, please use --with-ssl-lib-dir option or disabled the SSL support using --without-ssl])])
        else
            AC_MSG_RESULT([$ssllibdir])
        fi

        AC_DEFINE([HAVE_OPENSSL], 1, [Define to 1 if you have openssl.])
        AC_SUBST(sslincldir)
        AC_SUBST(ssllibdir)
    fi

    # Add SSL includes and libraries
    if test "$sslincldir" -a "$ssllibdir"
    then
        if test "x$ARCH" = "xDARWIN"; then
         # Darwin already knows about ssldirs
         LIBS="$LIBS -lssl -lcrypto"
        elif test -f "/usr/kerberos/include/krb5.h"; then
         # Redhat 9 compilation fix:
         CFLAGS="$CFLAGS -I$sslincldir -I/usr/kerberos/include"
         LIBS="$LIBS -L$ssllibdir -lssl -lcrypto"
        else
         CFLAGS="$CFLAGS -I$sslincldir"
         LIBS="$LIBS -L$ssllibdir -lssl -lcrypto"
        fi
    fi
fi
# Check TLS version
if test "$use_sslstatic" = "1" -o "$use_ssl" = "1"; then
        if test "x$sslincldir" != "x"; then
                CFLAGS="$CFLAGS -I$sslincldir"
        fi
        if test "x$ssllibdir" != "x"; then
                LIBS="$LIBS -L$ssllibdir"
        fi
        AC_MSG_CHECKING([for TLSv1.1])
        AC_LINK_IFELSE(
                [AC_LANG_PROGRAM([#include <openssl/ssl.h>], [TLSv1_1_method()])],
                [tlsv11=yes],
                [tlsv11=no])
        AC_MSG_RESULT($tlsv11)
        if test "$tlsv11" = "yes" ; then
                AC_DEFINE([HAVE_TLSV1_1], 1, [Define to 1 if you have openssl with TLSv1.1])
        fi

        AC_MSG_CHECKING([for TLSv1.2])
        AC_LINK_IFELSE(
                [AC_LANG_PROGRAM([#include <openssl/ssl.h>], [TLSv1_2_method()])],
                [tlsv12=yes],
                [tlsv12=no])
        AC_MSG_RESULT($tlsv12)
        if test "$tlsv12" = "yes" ; then
                AC_DEFINE([HAVE_TLSV1_2], 1, [Define to 1 if you have openssl with TLSv1.2])
        fi

        AC_MSG_CHECKING([for EC_KEY support])
        AC_LINK_IFELSE(
                [AC_LANG_PROGRAM([#include <openssl/ssl.h>], [EC_KEY_new_by_curve_name(0)])],
                [ec=yes],
                [ec=no])
        AC_MSG_RESULT($ec)
        if test "$ec" = "yes" ; then
                AC_DEFINE([HAVE_EC_KEY], 1, [Define to 1 if you have openssl with EC_KEY])
        fi

        AC_MSG_CHECKING([for ASN1_TIME_diff support])
        AC_LINK_IFELSE(
                [AC_LANG_PROGRAM([#include <openssl/ssl.h>], [ASN1_TIME_diff(0, 0, 0, 0)])],
                [asn1timediff=yes],
                [asn1timediff=no])
        AC_MSG_RESULT($asn1timediff)
        if test "$asn1timediff" = "yes" ; then
                AC_DEFINE([HAVE_ASN1_TIME_DIFF], 1, [Define to 1 if you have openssl with ASN1_TIME_diff])
        fi
fi


# ---------------------------------------------------------------------------
# Build options
# ---------------------------------------------------------------------------

AC_ARG_ENABLE(optimized,
        AS_HELP_STRING([--enable-optimized],
                [Build software optimized]),
    [
        CFLAGS=`echo $CFLAGS|sed 's/-O.//g'`
        if test "x$enableval" = "xyes" ; then
                CFLAGS=`echo $CFLAGS|sed 's/-g[[^ ]]*//g'`
                CFLAGS=`echo $CFLAGS|sed 's/-O.//g'`
                CFLAGS="$CFLAGS -O3 -DNDEBUG"
                OPTIMIZED=1
        else
                OPTIMIZED=0
        fi
    ],
    [
        OPTIMIZED=0
    ]
)

AC_ARG_ENABLE(profiling,
        AS_HELP_STRING([--enable-profiling],
                [Build with debug and profiling options]),
    [
        if test "x$enableval" = "xyes" ; then
                CFLAGS=`echo $CFLAGS|sed 's/-g.//g'`
                CFLAGS=`echo $CFLAGS|sed 's/-O.//g'`
                CFLAGS="$CFLAGS -g -pg"
                profile="true"
        else
                profile="false"
        fi
    ],
    [
        profile="false"
    ]
)
AM_CONDITIONAL([ENABLE_PROFILING], [test x$profile = xtrue])


# ------------------------------------------------------------------------
# Outputs
# ------------------------------------------------------------------------

AH_BOTTOM([
/* Mask out GCC __attribute__ extension for non-gcc compilers. */
#ifndef __GNUC__
#define __attribute__(x)
#endif
])

AC_CONFIG_HEADER([src/config.h])
AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([system/startup/monit.upstart])
AC_CONFIG_FILES([system/startup/monit.service])

AC_OUTPUT

echo
echo "Monit Build Information:"
echo
echo "                Architecture: ${ARCH}"
if test "$use_sslstatic" = "1" -o "$use_ssl" = "1"; then
echo "       SSL include directory: ${sslincldir}"
echo "       SSL library directory: ${ssllibdir}"
fi
echo "              Compiler flags: ${CFLAGS}"
echo "                Linker flags: ${LIBS}"
echo "           pid file location: ${piddir}"
echo "           Install directory: ${prefix}"
echo

cat <<EOT
+------------------------------------------------------------+
| License:                                                   |
| This is Open Source Software and use is subject to the GNU |
| AFFERO GENERAL PUBLIC LICENSE version 3, available in this |
| distribution in the file COPYING.                          |
|                                                            |
| By continuing this installation process, you are bound by  | 
| the terms of this license agreement. If you do not agree   |
| with the terms of this license, you must abort the         |
| installation process at this point.                        |
+------------------------------------------------------------+
| Monit has been configured with the following options:      |
|                                                            |
EOT
if test "$use_pam" = "1"; then
echo "|   PAM support:                                  ENABLED    |"
else
echo "|   PAM support:                                  DISABLED   |"
fi
if test "$use_sslstatic" = "1" -o "$use_ssl" = "1"; then
echo "|   SSL support:                                  ENABLED    |"
else
echo "|   SSL support:                                  DISABLED   |"
fi
if test "$use_largefiles" = "1"; then
echo "|   Large files support:                          ENABLED    |"
else
echo "|   Large files support:                          DISABLED   |"
fi
if test "x1" = "x$OPTIMIZED"; then
echo "|   Optimized:                                    ENABLED    |"
else
echo "|   Optimized:                                    DISABLED   |"
fi
if test "xfalse" = "x$profile"; then
echo "|   Profiling:                                    DISABLED   |"
else
echo "|   Profiling:                                    ENABLED    |"
fi
echo "+------------------------------------------------------------+"
