#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_INIT([lxc-templates], [3.0.4])
AM_INIT_AUTOMAKE

# We need pkg-config
PKG_PROG_PKG_CONFIG

AC_CONFIG_SRCDIR([configure.ac])
# AC_CONFIG_AUX_DIR([config])
# AC_CONFIG_HEADERS([src/config.h])
# AM_INIT_AUTOMAKE([-Wall -Werror -Wno-portability subdir-objects])
AC_CANONICAL_HOST
# AM_PROG_CC_C_O
AC_GNU_SOURCE

# Detect the distribution. This is used for the default configuration and
# for some distro-specific build options.
AC_MSG_CHECKING([host distribution])
AC_ARG_WITH(distro, AS_HELP_STRING([--with-distro=DISTRO], [Specify the Linux distribution to target: One of redhat, oracle, centos, fedora, suse, gentoo, debian, arch, slackware, plamo, paldo, openmandriva, pardus, sparclinux, altlinux.]))
if type lsb_release >/dev/null 2>&1 && test "z$with_distro" = "z"; then
	with_distro=`lsb_release -is`
fi
if test "z$with_distro" = "z"; then
	AC_CHECK_FILE(/etc/redhat-release,with_distro="redhat")
	AC_CHECK_FILE(/etc/oracle-release,with_distro="oracle")
	AC_CHECK_FILE(/etc/sparclinux-release,with_distro="sparclinux")
	AC_CHECK_FILE(/etc/centos-release,with_distro="centos")
	AC_CHECK_FILE(/etc/fedora-release,with_distro="fedora")
	AC_CHECK_FILE(/etc/SuSE-release,with_distro="suse")
	AC_CHECK_FILE(/etc/gentoo-release,with_distro="gentoo")
	AC_CHECK_FILE(/etc/debian_version,with_distro="debian")
	AC_CHECK_FILE(/etc/arch-release,with_distro="arch")
	AC_CHECK_FILE(/etc/slackware-version,with_distro="slackware")
	AC_CHECK_FILE(/etc/plamo-version,with_distro="plamo")
	AC_CHECK_FILE(/etc/frugalware-release,with_distro="frugalware")
	AC_CHECK_FILE(/etc/mandrakelinux-release, with_distro="openmandriva")
	AC_CHECK_FILE(/etc/mandriva-release,with_distro="openmandriva")
	AC_CHECK_FILE(/etc/pardus-release,with_distro="pardus")
	AC_CHECK_FILE(/etc/altlinux-release,with_distro="altlinux")
	AC_CHECK_FILE(/etc/pld-release,with_distro="pld")
fi
with_distro=`echo ${with_distro} | tr '[[:upper:]]' '[[:lower:]]'`

if test "z$with_distro" = "zforsparc"; then
	with_distro="sparclinux"
fi

if test "z$with_distro" = "z"; then
	with_distro="unknown"
fi
case $with_distro in
	ubuntu|raspbian)
		distrosysconf="$sysconfdir/default"
		;;
	redhat|centos|fedora|oracle|oracleserver|sparclinux|altlinux|suse|opensuse*|plamo|pld)
		distrosysconf="$sysconfdir/sysconfig"
		;;
	*)
		distrosysconf="$sysconfdir/default"
		;;
esac
AC_MSG_RESULT([$with_distro])
AM_CONDITIONAL([HAVE_DEBIAN], [test x"$with_distro" = "xdebian" -o x"$with_distro" = "xubuntu" -o x"$with_distro" = "xraspbian"])
AM_CONDITIONAL([DISTRO_UBUNTU], [test "x$with_distro" = "xubuntu"])

AC_CONFIG_LINKS([config/etc/default.conf:config/etc/${distroconf}])

# Not in older autoconf versions
# AS_VAR_COPY(DEST, SOURCE)
# -------------------------
# Set the polymorphic shell variable DEST to the contents of the polymorphic
# shell variable SOURCE.
m4_ifdef([AS_VAR_COPY], [],
[AC_DEFUN([AS_VAR_COPY],
    [AS_LITERAL_IF([$1[]$2], [$1=$$2], [eval $1=\$$2])])
])

dnl PKG_CHECK_VAR was introduced with pkg-config 0.28
m4_ifdef([PKG_CHECK_VAR], [],
[AC_DEFUN([PKG_CHECK_VAR],
    [AC_REQUIRE([PKG_PROG_PKG_CONFIG])dnl
     AC_ARG_VAR([$1], [value of $3 for $2, overriding pkg-config])dnl
     _PKG_CONFIG([$1], [variable="][$3]["], [$2])
     AS_VAR_COPY([$1], [pkg_cv_][$1])
     AS_VAR_IF([$1], [""], [$5], [$4])dnl
    ])# PKG_CHECK_VAR
])

# Allow overriding the default runtime dir (/run)
AC_ARG_WITH([runtime-path],
	[AC_HELP_STRING(
		[--with-runtime-path=dir],
		[runtime directory (default: /run)]
	)], [], [with_runtime_path=['/run']])

# LXC container path, where the containers are actually stored
# This is overridden by an entry in the file called LXCCONF
# (i.e. /etc/lxc/lxc.conf)
AC_ARG_WITH([config-path],
	[AC_HELP_STRING(
		[--with-config-path=dir],
		[lxc configuration repository path]
	)], [], [with_config_path=['${localstatedir}/lib/lxc']])

# The path of the global lxc configuration file.
AC_ARG_WITH([global-conf],
	[AC_HELP_STRING(
		[--with-global-conf=dir],
		[global lxc configuration file]
	)], [], [with_global_conf=['${sysconfdir}/lxc/lxc.conf']])

# Rootfs path, where the container mount structure is assembled
AC_ARG_WITH([rootfs-path],
	[AC_HELP_STRING(
		[--with-rootfs-path=dir],
		[lxc rootfs mount point]
	)], [], [with_rootfs_path=['${libdir}/lxc/rootfs']])

# Expand some useful variables
AS_AC_EXPAND(PREFIX, "$prefix")
AS_AC_EXPAND(LIBDIR, "$libdir")
AS_AC_EXPAND(BINDIR, "$bindir")
AS_AC_EXPAND(SBINDIR, "$sbindir")
AS_AC_EXPAND(LIBEXECDIR, "$libexecdir")
AS_AC_EXPAND(INCLUDEDIR, "$includedir")
AS_AC_EXPAND(SYSCONFDIR, "$sysconfdir")
AS_AC_EXPAND(LXC_DEFAULT_CONFIG, "$sysconfdir/lxc/default.conf")
AS_AC_EXPAND(DATADIR, "$datadir")
AS_AC_EXPAND(LOCALSTATEDIR, "$localstatedir")
AS_AC_EXPAND(LXC_GENERATE_DATE, "$(date --utc --date=@${SOURCE_DATE_EPOCH:-$(date +%s)} '+%Y-%m-%d')")
AS_AC_EXPAND(LXCPATH, "$with_config_path")
AS_AC_EXPAND(LXC_GLOBAL_CONF, "$with_global_conf")
AS_AC_EXPAND(LXC_USERNIC_CONF, "$with_usernic_conf")
AS_AC_EXPAND(LXC_USERNIC_DB, "$with_usernic_db")
AS_AC_EXPAND(LXC_DISTRO_SYSCONF, "$distrosysconf")
AS_AC_EXPAND(LXCROOTFSMOUNT, "$with_rootfs_path")
AS_AC_EXPAND(LXCTEMPLATEDIR, "$datadir/lxc/templates")
AS_AC_EXPAND(LXCTEMPLATECONFIG, "$datadir/lxc/config")
AS_AC_EXPAND(LXCHOOKDIR, "$datadir/lxc/hooks")
AS_AC_EXPAND(LXCBINHOOKDIR, "$libexecdir/lxc/hooks")
AS_AC_EXPAND(LXCINITDIR, "$libexecdir")
AS_AC_EXPAND(RUNTIME_PATH, "$with_runtime_path")

# Check for some standard binaries
AC_PROG_SED

# Files requiring some variable expansion
AC_CONFIG_FILES([
	Makefile

	config/Makefile
	config/alpine.common.conf
	config/alpine.userns.conf
	config/archlinux.common.conf
	config/archlinux.userns.conf
	config/centos.common.conf
	config/centos.userns.conf
	config/debian.common.conf
	config/debian.userns.conf
	config/fedora.common.conf
	config/fedora.userns.conf
	config/gentoo.common.conf
	config/gentoo.moresecure.conf
	config/gentoo.userns.conf
	config/opensuse.common.conf
	config/opensuse.userns.conf
	config/oracle.common.conf
	config/oracle.userns.conf
	config/plamo.common.conf
	config/plamo.userns.conf
	config/slackware.common.conf
	config/slackware.userns.conf
	config/ubuntu-cloud.common.conf
	config/ubuntu-cloud.lucid.conf
	config/ubuntu-cloud.userns.conf
	config/ubuntu.common.conf
	config/ubuntu.lucid.conf
	config/ubuntu.userns.conf
	config/openwrt.common.conf
	config/sparclinux.common.conf
	config/sparclinux.userns.conf
	config/voidlinux.common.conf
	config/voidlinux.userns.conf
	config/sabayon.common.conf
	config/sabayon.userns.conf

	templates/Makefile
	templates/lxc-alpine
	templates/lxc-altlinux
	templates/lxc-archlinux
	templates/lxc-centos
	templates/lxc-cirros
	templates/lxc-debian
	templates/lxc-fedora
	templates/lxc-fedora-legacy
	templates/lxc-gentoo
	templates/lxc-openmandriva
	templates/lxc-opensuse
	templates/lxc-oracle
	templates/lxc-plamo
	templates/lxc-pld
	templates/lxc-slackware
	templates/lxc-sshd
	templates/lxc-ubuntu
	templates/lxc-ubuntu-cloud
	templates/lxc-sparclinux
	templates/lxc-voidlinux
	templates/lxc-sabayon
])
AC_CONFIG_COMMANDS([default],[[]],[[]])
AC_OUTPUT

# Configuration overview
cat << EOF

----------------------------
Environment:
 - distribution: $with_distro
EOF
