# Makefile for rpm library.

include $(top_srcdir)/rpm.am

AM_CPPFLAGS = -I$(top_builddir) -I$(top_srcdir) -I$(top_builddir)/include/
AM_CPPFLAGS += @WITH_NSS_INCLUDE@
AM_CPPFLAGS += @WITH_POPT_INCLUDE@
AM_CPPFLAGS += -I$(top_srcdir)/misc
AM_CPPFLAGS += -DLOCALEDIR="\"$(localedir)\""
AM_CPPFLAGS += -DSYSCONFDIR="\"$(sysconfdir)\""
AM_CPPFLAGS += -DLOCALSTATEDIR="\"$(localstatedir)\""
AM_CPPFLAGS += -DLIBRPMALIAS_FILENAME="\"rpmpopt-${VERSION}\""
AM_CPPFLAGS += -DLIBRPMALIAS_EXECPATH="\"$(bindir)\""

usrlibdir = $(libdir)

check_PROGRAMS =
CLEANFILES =
EXTRA_DIST = gentagtbl.sh tagtbl.C rpmhash.C rpmhash.H
EXTRA_PROGRAMS =

usrlib_LTLIBRARIES = librpm.la
librpm_la_SOURCES = \
	backend/dbconfig.c backend/db3.c backend/dbi.h \
	headerutil.c header.c headerfmt.c header_internal.h \
	rpmdb.c rpmdb_internal.h \
	fprint.c fprint.h tagname.c rpmtd.c \
	cpio.c cpio.h depends.c order.c formats.c tagexts.c fsm.c fsm.h \
	manifest.c manifest.h misc.c package.c \
	poptALL.c poptI.c poptQV.c psm.c query.c \
	rpmal.c rpmal.h rpmchecksig.c rpmds.c rpmfi.c rpmfi_internal.h \
	rpmgi.h rpmgi.c rpminstall.c rpmts_internal.h \
	rpmlead.c rpmlead.h rpmps.c rpmprob.c rpmrc.c \
	rpmte.c rpmte_internal.h rpmts.c rpmfs.h rpmfs.c \
	rpmvercmp.c signature.c signature.h transaction.c \
	verify.c rpmlock.c rpmlock.h misc.h \
	rpmscript.h rpmscript.c legacy.c merge.c \
	rpmchroot.c rpmchroot.h \
	rpmplugins.c rpmplugins.h rpmug.c rpmug.h

librpm_la_LDFLAGS = -version-info 2:2:0

librpm_la_LIBADD = \
	$(top_builddir)/rpmio/librpmio.la \
	@WITH_POPT_LIB@ \
	@WITH_SELINUX_LIB@ \
	@WITH_CAP_LIB@ \
	@WITH_ACL_LIB@ \
	@LIBINTL@

if WITH_LUA
AM_CPPFLAGS += @LUA_CFLAGS@
librpm_la_LIBADD += @LUA_LIBS@
librpm_la_SOURCES += rpmliblua.c rpmliblua.h
endif

if WITH_INTERNAL_DB
librpm_la_LIBADD += $(libdb_la)
else
librpm_la_LIBADD += @WITH_DB_LIB@
endif

tagtbl.C: Makefile.am $(srcdir)/rpmtag.h gentagtbl.sh
	@AWK=${AWK} ${SHELL} $(srcdir)/gentagtbl.sh \
		    	     $(srcdir)/rpmtag.h > $@.new && \
	 mv -f $@.new $@
BUILT_SOURCES = tagtbl.C

if WITH_INTERNAL_DB
# XXX watchout, $(top_builddir)/db3/libdb.la created by this Makefile may surprise
libdb_la = $(top_builddir)/db3/libdb.la

# XXX grrr, force noinst libdb.la for db3.
# there are more reliable ways to get the BDB version info, just a dirty
# hack for now...
BDBVER = $(shell grep ^LIBVERSION $(top_builddir)/db3/Makefile|cut -f2)
$(libdb_la): $(top_builddir)/db3/libdb-$(BDBVER).la
	sed -e"/^libdir=/s/^.*$$/libdir=''/" \
	  < $(top_builddir)/db3/libdb-$(BDBVER).la > $(libdb_la)
CLEANFILES += $(libdb_la)


rpmlibexec_PROGRAMS =

rpmlibexec_PROGRAMS += rpmdb_archive
rpmdb_archive_SOURCES =
rpmdb_archive_LDADD = \
	$(top_builddir)/db3/db_archive.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_checkpoint
rpmdb_checkpoint_SOURCES =
rpmdb_checkpoint_LDADD = \
	$(top_builddir)/db3/db_checkpoint.o \
	$(top_builddir)/db3/util_log.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_deadlock
rpmdb_deadlock_SOURCES =
rpmdb_deadlock_LDADD = \
	$(top_builddir)/db3/db_deadlock.o \
	$(top_builddir)/db3/util_log.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_dump
rpmdb_dump_SOURCES =
rpmdb_dump_LDADD = \
	$(top_builddir)/db3/db_dump.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_load
rpmdb_load_SOURCES =
rpmdb_load_LDADD = \
	$(top_builddir)/db3/db_load.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_printlog
rpmdb_printlog_SOURCES =
rpmdb_printlog_LDADD = \
	$(top_builddir)/db3/db_printlog.o \
	$(top_builddir)/db3/btree_autop.o \
	$(top_builddir)/db3/crdel_autop.o \
	$(top_builddir)/db3/db_autop.o \
	$(top_builddir)/db3/dbreg_autop.o \
	$(top_builddir)/db3/fileops_autop.o \
	$(top_builddir)/db3/hash_autop.o \
	$(top_builddir)/db3/qam_autop.o \
	$(top_builddir)/db3/rep_autop.o \
	$(top_builddir)/db3/txn_autop.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_recover
rpmdb_recover_SOURCES =
rpmdb_recover_LDADD = \
	$(top_builddir)/db3/db_recover.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_stat
rpmdb_stat_SOURCES =
rpmdb_stat_LDADD = \
	$(top_builddir)/db3/db_stat.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_upgrade
rpmdb_upgrade_SOURCES =
rpmdb_upgrade_LDADD = \
	$(top_builddir)/db3/db_upgrade.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la

rpmlibexec_PROGRAMS += rpmdb_verify 
rpmdb_verify_SOURCES =
rpmdb_verify_LDADD = \
	$(top_builddir)/db3/db_verify.o \
	$(top_builddir)/db3/util_cache.o \
	$(top_builddir)/db3/util_sig.o \
	librpm.la
endif

CLEANFILES += $(BUILT_SOURCES)
