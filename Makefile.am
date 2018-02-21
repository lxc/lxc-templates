# Makefile.am

ACLOCAL_AMFLAGS = -I config

LIBTOOL_DEPS = @LIBTOOL_DEPS@
SUBDIRS = config templates
DIST_SUBDIRS = config templates
EXTRA_DIST = \
	autogen.sh \
	CONTRIBUTING \
	MAINTAINERS

install-data-local:
	$(MKDIR_P) $(DESTDIR)$(LXCPATH)
	$(MKDIR_P) $(DESTDIR)$(localstatedir)/cache/lxc

ChangeLog::
	@touch ChangeLog
