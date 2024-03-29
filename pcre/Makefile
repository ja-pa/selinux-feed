#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pcre
PKG_VERSION:=8.42
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/$(PKG_NAME)
PKG_HASH:=2cd04b7c887808be030254e8d77de11d3fe9d4505c39d4b15d2664ffe8bf9301
PKG_MAINTAINER:=Thomas Heil <heil@terminal-consulting.de>

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENCE

PKG_FIXUP:=autoreconf

PKG_INSTALL:=1

include $(INCLUDE_DIR)/uclibc++.mk
include $(INCLUDE_DIR)/package.mk

define Package/libpcre/default
  SECTION:=libs
  CATEGORY:=Libraries
  URL:=https://www.pcre.org/
  DEPENDS:=$(CXX_DEPENDS)
endef

define Package/libpcre
  $(call Package/libpcre/default)
  TITLE:=A Perl Compatible Regular Expression library
endef

define Package/libpcre16
  $(call Package/libpcre/default)
  TITLE:=A Perl Compatible Regular Expression library (16bit support)
endef

define Package/libpcrecpp
  $(call Package/libpcre/default)
  TITLE:=C++ wrapper for Perl Compatible Regular Expression library
  DEPENDS:=+libpcre
endef

include $(INCLUDE_DIR)/host-build.mk

HOST_CONFIGURE_ARGS += \
	--enable-utf8 \
	--enable-unicode-properties \
	--enable-pcre16 \
	--with-match-limit-recursion=16000 \
	--enable-cpp

$(eval $(call HostBuild))

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-utf8 \
	--enable-unicode-properties \
	--enable-pcre16 \
	--with-match-limit-recursion=16000 \
	--enable-cpp


MAKE_FLAGS += \
	CFLAGS="$(TARGET_CFLAGS)"

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/pcre-config $(1)/usr/bin/

	$(INSTALL_DIR) $(2)/bin
	$(LN) $(STAGING_DIR)/usr/bin/pcre-config $(2)/bin

	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/pcre*.h $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpcre*.{a,so*} $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libpcre*.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libpcre/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpcre{,posix}.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpcre.so $(1)/usr/lib/
endef

define Package/libpcre16/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpcre16.so* $(1)/usr/lib/
endef

define Package/libpcrecpp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpcrecpp.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libpcre))
$(eval $(call BuildPackage,libpcre16))
$(eval $(call BuildPackage,libpcrecpp))
