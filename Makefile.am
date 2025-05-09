@SET_MAKE@

AUTOMAKE_OPTIONS = subdir-objects
ACLOCAL_AMFLAGS = -I m4 ${ACLOCAL_FLAGS}

SUBDIRS = po

BUILT_SOURCES =
EXTRA_DIST =
DISTCLEANFILES =
noinst_LTLIBRARIES =

bin_PROGRAMS = src/xfce4-screenshooter

distclean-local:
	rm -rf *.cache *~

distuninstallcheck_listfiles = \
	find . -type f -print | grep -v ./share/icons/hicolor/icon-theme.cache

dist-bz2: dist
	zcat $(PACKAGE)-$(VERSION).tar.gz | bzip2 --best -c > $(PACKAGE)-$(VERSION).tar.bz2

distcheck-bz2: distcheck
	zcat $(PACKAGE)-$(VERSION).tar.gz | bzip2 --best -c > $(PACKAGE)-$(VERSION).tar.bz2

# Wayland protocols
if ENABLE_WAYLAND

noinst_LTLIBRARIES += \
	protocols/libprotocols.la

protocols_libprotocols_built_sources = \
	protocols/wlr-screencopy-unstable-v1.c \
	protocols/wlr-screencopy-unstable-v1-client.h

nodist_protocols_libprotocols_la_SOURCES = \
	protocols/wlr-screencopy-unstable-v1.c

protocols_libprotocols_la_CFLAGS = \
	$(WAYLAND_CLIENT_CFLAGS) \
	$(PLATFORM_CFLAGS)

protocols_libprotocols_la_LDFLAGS = \
	-no-undefined \
	$(PLATFORM_LDFLAGS)

protocols_libprotocols_la_LIBADD = \
	$(WAYLAND_CLIENT_LIBS)

protocols/%.c: $(WLR_PROTOCOLS_PKGDATADIR)/unstable/%.xml
	$(AM_V_GEN) wayland-scanner private-code $< $@

protocols/%-client.h: $(WLR_PROTOCOLS_PKGDATADIR)/unstable/%.xml
	$(AM_V_GEN) wayland-scanner client-header $< $@

BUILT_SOURCES += \
	$(protocols_libprotocols_built_sources)

DISTCLEANFILES += \
	$(protocols_libprotocols_built_sources)
endif

EXTRA_DIST += \
	protocols/wlr-protocols/unstable/wlr-screencopy-unstable-v1.xml

# Convienence library for the application and the panel plugin
noinst_LTLIBRARIES += lib/libscreenshooter.la

lib_libscreenshooter_la_SOURCES = \
	$(lib_libscreenshooter_built_sources) \
	lib/libscreenshooter.h \
	lib/screenshooter-actions.c lib/screenshooter-actions.h \
	lib/screenshooter-capture.c lib/screenshooter-capture.h \
	lib/screenshooter-custom-actions.c lib/screenshooter-custom-actions.h \
	lib/screenshooter-format.c lib/screenshooter-format.h \
	lib/screenshooter-dialogs.c lib/screenshooter-dialogs.h \
	lib/screenshooter-global.h \
	lib/screenshooter-utils.c lib/screenshooter-utils.h

if ENABLE_X11
lib_libscreenshooter_la_SOURCES += \
	lib/screenshooter-capture-x11.c lib/screenshooter-capture-x11.h \
	lib/screenshooter-utils-x11.c lib/screenshooter-utils-x11.h
endif

if ENABLE_WAYLAND
lib_libscreenshooter_la_SOURCES += \
	lib/screenshooter-capture-wayland.c lib/screenshooter-capture-wayland.h
endif

lib_libscreenshooter_la_CFLAGS = \
	-I$(top_srcdir) \
	-I$(top_builddir)/lib \
	@EXO_CFLAGS@ \
	@GTK_CFLAGS@ \
	@GDK_CFLAGS@ \
	@GDKX11_CFLAGS@ \
	@GLIB_CFLAGS@ \
	@LIBXFCE4UTIL_CFLAGS@ \
	@LIBXFCE4UI_CFLAGS@ \
	@LIBX11_CFLAGS@ \
	@WAYLAND_CLIENT_CFLAGS@ \
	@XFIXES_CFLAGS@ \
	@XFCONF_CFLAGS@ \
	-DPACKAGE_LOCALE_DIR=\"$(localedir)\"

lib_libscreenshooter_la_LIBADD = \
	-lm \
	@EXO_LIBS@ \
	@GTK_LIBS@ \
	@GDK_LIBS@ \
	@GDKX11_LIBS@ \
	@LIBXFCE4UTIL_LIBS@ \
	@LIBXFCE4UI_LIBS@ \
	@GLIB_LIBS@ \
	@LIBXEXT_LIBS@ \
	@LIBX11_LIBS@ \
	@WAYLAND_CLIENT_LIBS@ \
	@XFCONF_LIBS@ \
	@XFIXES_LIBS@

if ENABLE_WAYLAND
lib_libscreenshooter_la_LIBADD += \
	$(top_builddir)/protocols/libprotocols.la
endif

lib_libscreenshooter_built_sources = \
	lib/screenshooter-marshal.c lib/screenshooter-marshal.h

#Autogenerated sources for the library
BUILT_SOURCES += $(lib_libscreenshooter_built_sources)

lib/screenshooter-marshal.h: lib/stamp-screenshooter-marshal.h
	@true
lib/stamp-screenshooter-marshal.h: lib/screenshooter-marshal.list Makefile
	$(AM_V_GEN) ( \
		echo '/* this file is autogenerated -- do not edit */' >lib/screenshooter-marshal.h \
		&& glib-genmarshal --prefix=_screenshooter_marshal --header $(top_srcdir)/lib/screenshooter-marshal.list >>lib/screenshooter-marshal.h \
		&& echo timestamp >$@ \
	)

lib/screenshooter-marshal.c: lib/screenshooter-marshal.list Makefile
	$(AM_V_GEN) ( \
		echo '/* this file is autogenerated -- do not edit */' >$@ \
		&& echo '#include "screenshooter-marshal.h"' >>$@ \
		&& glib-genmarshal --prefix=_screenshooter_marshal --body $(top_srcdir)/lib/screenshooter-marshal.list >>$@ \
	)

# Main application
src_xfce4_screenshooter_CFLAGS = \
	-I$(top_srcdir)/lib/ \
	-I$(top_builddir)/lib \
	@EXO_CFLAGS@ \
	@GTK_CFLAGS@ \
	@GDK_CFLAGS@ \
	@GDKX11_CFLAGS@ \
	@GLIB_CFLAGS@ \
	@LIBXFCE4UTIL_CFLAGS@ \
	@LIBXFCE4UI_CFLAGS@ \
	@GTHREAD_CFLAGS@ \
	@LIBX11_CFLAGS@ \
	-DPACKAGE_LOCALE_DIR=\"$(localedir)\"

src_xfce4_screenshooter_LDFLAGS = \
	@EXO_LIBS@ \
	@GTK_LIBS@ \
	@GDK_LIBS@ \
	@GDKX11_LIBS@ \
	@GLIB_LIBS@ \
	@GTHREAD_LIBS@ \
	@LIBX11_LIBS@ \
	@LIBXFCE4UTIL_LIBS@ \
	@LIBXFCE4UI_LIBS@

src_xfce4_screenshooter_LDADD = lib/libscreenshooter.la

src_xfce4_screenshooter_SOURCES = src/main.c

if ENABLE_WAYLAND
src_xfce4_screenshooter_CFLAGS += \
	-I$(top_srcdir)/protocols/ \
	-I$(top_builddir)/protocols
endif

# Manual file for the application
if HAVE_HELP2MAN
xfce4-screenshooter.1: src/xfce4-screenshooter
	$(HELP2MAN) -o $@ --no-info $<
endif

# Desktop file for the application
app_desktopdir = $(datadir)/applications
app_desktop_in_files = src/xfce4-screenshooter.desktop.in
app_desktop_DATA = $(app_desktop_in_files:.desktop.in=.desktop)

src/xfce4-screenshooter.desktop: src/xfce4-screenshooter.desktop.in
	$(AM_V_GEN)$(MSGFMT) --desktop --template $< -d $(top_srcdir)/po -o $@

# Panel plugin
plugindir = $(libdir)/xfce4/panel/plugins
plugin_LTLIBRARIES = panel-plugin/libscreenshooterplugin.la

panel_plugin_libscreenshooterplugin_la_CFLAGS =	\
	-DPACKAGE_LOCALE_DIR=\"$(localedir)\"	\
	-I$(top_srcdir)	\
	-I$(top_srcdir)/lib/	\
	-I$(top_builddir)/lib/	\
	@EXO_CFLAGS@ \
	@LIBXFCE4PANEL_CFLAGS@ \
	@LIBXFCE4UI_CFLAGS@ \
	@GTHREAD_CFLAGS@ \
	@LIBX11_CFLAGS@

panel_plugin_libscreenshooterplugin_la_LDFLAGS = \
	-avoid-version \
	-module \
	-no-undefined \
	-export-symbols-regex '^xfce_panel_module_(preinit|init|construct)' \
	$(PLATFORM_LDFLAGS)

panel_plugin_libscreenshooterplugin_la_LIBADD = \
	@EXO_LIBS@ \
	@LIBXFCE4PANEL_LIBS@ \
	@GTHREAD_LIBS@ \
	@LIBX11_LIBS@ \
	@LIBXFCE4UI_LIBS@ \
	lib/libscreenshooter.la

panel_plugin_libscreenshooterplugin_la_SOURCES = \
	panel-plugin/screenshooter-plugin.c

# Panel plugin desktop file
panel_desktopdir = $(datadir)/xfce4/panel/plugins
panel_desktop_in_files = panel-plugin/screenshooter.desktop.in
panel_desktop_DATA = $(panel_desktop_in_files:.desktop.in=.desktop)

panel-plugin/screenshooter.desktop: panel-plugin/screenshooter.desktop.in
	$(AM_V_GEN)$(MSGFMT) --desktop --template $< -d $(top_srcdir)/po -o $@

# appdata file for the application
appdatadir = $(datadir)/metainfo
appdata_DATA = $(appdata_in_files:.xml.in=.xml)
appdata_in_files = src/xfce4-screenshooter.appdata.xml.in

src/xfce4-screenshooter.appdata.xml: src/xfce4-screenshooter.appdata.xml.in
	$(AM_V_GEN)$(MSGFMT) --xml --template $< -d $(top_srcdir)/po -o $@

# App icons

# 16x16 icons
16iconsdir = $(datadir)/icons/hicolor/16x16/apps
16icons_DATA = icons/16x16/org.xfce.screenshooter.png

# 24x24 icons
24iconsdir = $(datadir)/icons/hicolor/24x24/apps
24icons_DATA = icons/24x24/org.xfce.screenshooter.png

# 32x32 icons
32iconsdir = $(datadir)/icons/hicolor/32x32/apps
32icons_DATA = icons/32x32/org.xfce.screenshooter.png

# 48x48 icons
48iconsdir = $(datadir)/icons/hicolor/48x48/apps
48icons_DATA = icons/48x48/org.xfce.screenshooter.png

# 128x128 icons
128iconsdir = $(datadir)/icons/hicolor/128x128/apps
128icons_DATA = icons/128x128/org.xfce.screenshooter.png

# Scalable icons
scaliconsdir = $(datadir)/icons/hicolor/scalable/apps
scalicons_DATA = icons/scalable/org.xfce.screenshooter.svg

# Update the icon cache
gtk_update_icon_cache = gtk-update-icon-cache -f -t $(datadir)/icons/hicolor

install-data-hook:
	@-if test -z "$(DESTDIR)"; then								\
		echo "Updating Gtk icon cache.";						\
		$(gtk_update_icon_cache);								\
	else														\
		echo "*** Icon cache not updated. Remember to run:";	\
		echo "***";												\
		echo "***   $(gtk_update_icon_cache)";					\
		echo "***";												\
	fi

.PHONY: ChangeLog

ChangeLog: Makefile
	(GIT_DIR=$(top_srcdir)/.git git log > .changelog.tmp \
	&& mv .changelog.tmp ChangeLog; rm -f .changelog.tmp) \
	|| (touch ChangeLog; echo 'Git directory not found: installing possibly empty changelog.' >&2)

dist-hook: ChangeLog

# Extra dist and distclean rules
EXTRA_DIST += \
	lib/screenshooter-marshal.list \
	meson.build \
	meson_options.txt \
	icons/meson.build \
	lib/meson.build \
	panel-plugin/meson.build \
	po/meson.build \
	protocols/meson.build \
	src/meson.build \
	xfce-revision.h.in \
	$(app_desktop_in_files) \
	$(panel_desktop_in_files) \
	$(16icons_DATA) \
	$(24icons_DATA) \
	$(32icons_DATA) \
	$(48icons_DATA) \
	$(128icons_DATA) \
	$(scalicons_DATA) \
	$(appdata_in_files)

DISTCLEANFILES += \
	stamp-screenshooter-marshal.h \
	$(lib_libscreenshooter_built_sources) \
	lib/stamp-screenshooter-marshal.h \
	$(app_desktop_DATA) \
	$(panel_desktop_DATA) \
	$(appdata_DATA) \
	xfce4-screenshooter.1

# Man pages
dist_man_MANS = xfce4-screenshooter.1
