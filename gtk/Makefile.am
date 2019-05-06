lib_LTLIBRARIES = libzbargtk.la
libzbargtk_la_CPPFLAGS = -I$(srcdir) $(GTK_CFLAGS) $(AM_CPPFLAGS)
libzbargtk_la_LDFLAGS = -version-info $(ZGTK_LIB_VERSION) \
    -export-symbols-regex "^zbar_.*" $(AM_LDFLAGS) -no-undefined
libzbargtk_la_LIBADD = $(GTK_LIBS) ../zbar/libzbar.la $(AM_LIBADD)

if HAVE_X
libzbargtk_la_CPPFLAGS += -DHAVE_X
endif

dist_libzbargtk_la_SOURCES = zbargtk.c zbargtkprivate.h
nodist_libzbargtk_la_SOURCES = zbarmarshal.c zbarmarshal.h
BUILT_SOURCES = zbarmarshal.c zbarmarshal.h
CLEANFILES = $(BUILT_SOURCES)
EXTRA_DIST = zbarmarshal.list

%.h: %.list
	$(GLIB_GENMARSHAL) --g-fatal-warnings --prefix=zbar_marshal \
	    --header $^ > $@

%.c: %.list
	$(GLIB_GENMARSHAL) --g-fatal-warnings --prefix=zbar_marshal \
	    --body $^ > $@

../zbar/libzbar.la:
	$(MAKE) -C $(abs_top_srcdir) zbar/libzbar.la

# GObject Introspection

include $(INTROSPECTION_MAKEFILE)

ZBarGtk-1.0.gir: $(lib_LTLIBRARIES)

INTROSPECTION_SCANNER_ARGS = --identifier-prefix=zbar_gtk \
			     --identifier-prefix=ZBarGtk

INTROSPECTION_GIRS = ZBarGtk-1.0.gir
ZBarGtk_1_0_gir_NAMESPACE = ZBarGtk
ZBarGtk_1_0_gir_VERSION = 1.0
ZBarGtk_1_0_gir_LIBS = $(lib_LTLIBRARIES)
ZBarGtk_1_0_gir_FILES = $(libzbargtk_la_SOURCES) \
			../include/zbar/zbargtk.h \
			zbargtk.c
ZBarGtk_1_0_gir_INCLUDES = GObject-2.0 Gtk-3.0 Gdk-3.0
ZBarGtk_1_0_gir_CFLAGS= $(libzbargtk_la_CPPFLAGS)

if HAVE_INTROSPECTION

girdir = $(INTROSPECTION_GIRDIR)
dist_gir_DATA = $(INTROSPECTION_GIRS)
typelibdir = $(INTROSPECTION_TYPELIBDIR)
typelib_DATA = ZBarGtk-1.0.typelib

CLEANFILES += $(dist_gir_DATA) $(typelib_DATA)

endif
