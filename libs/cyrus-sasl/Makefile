#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cyrus-sasl
PKG_VERSION:=2.1.28
PKG_RELEASE:=4

PKG_MAINTAINER:=W. Michael Petullo <mike@flyn.org>

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/cyrusimap/cyrus-sasl/releases/download/cyrus-sasl-2.1.28/
PKG_HASH:=7ccfc6abd01ed67c1a0924b353e526f1b766b21f42d4562ee635a8ebfc5bb38c

PKG_LICENSE:=BSD-4c BSD
PKG_LICENSE_FILES:=COPYING cmulocal/COPYING saslauthd/COPYING
PKG_CPE_ID:=cpe:/a:cmu:cyrus-sasl

PKG_FIXUP:=autoreconf
PKG_MACRO_PATHS:=cmulocal config ../cmulocal ../config
PKG_AUTOMAKE_PATHS:=. saslauthd sasldb
PKG_REMOVE_FILES:=aclocal.m4 saslauthd/aclocal.m4 config/libtool.m4

include $(INCLUDE_DIR)/package.mk

define Package/libsasl2/Default
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=A general purpose authentication library
  URL:=http://asg.web.cmu.edu/sasl/
endef

define Package/libsasl2
  $(call Package/libsasl2/Default)
  DEPENDS:=+libopenssl
  TITLE+= (libraries)
endef

define Package/libsasl2-sasldb
  $(call Package/libsasl2/Default)
  DEPENDS:=+libsasl2 +libdb47
  TITLE+= (sasldb libraries)
endef

define Package/libsasl2-utils
  $(call Package/libsasl2/Default)
  DEPENDS:=+libsasl2 +libdb47
  TITLE+= (sasldb utilities)
endef

TARGET_CFLAGS += $(FPIC)
CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-sample \
	--enable-staticdlopen \
	--disable-java \
	--disable-alwaystrue \
	--disable-checkapop \
	--enable-cram \
	--enable-digest \
	--enable-auth-sasldb \
	--disable-otp \
	--disable-srp \
	--disable-srp-setpass \
	--disable-krb4 \
	--disable-gssapi \
	--disable-gss_mutexes \
	--enable-plain \
	--enable-anon \
	--disable-login \
	--disable-ntlm \
	--disable-sql \
	--disable-ldapdb \
	--with-dblib=berkeley \
	--without-gdbm \
	--with-devrandom="/dev/urandom" \
	--without-pam \
	--without-saslauthd \
	--without-authdaemond \
	--without-pwcheck \
	--with-ipctype=unix \
	--with-openssl="$(STAGING_DIR)/usr" \
	--without-des \
	--without-opie \
	--without-ldap \
	--without-mysql \
	--without-pgsql \
	--without-sqlite \
	--without-rc4 \
	--without-dmalloc \
	--without-sfio \
	--disable-sample

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/include \
		CC="$(HOSTCC)" \
		LINK="$(HOSTCC) -o makemd5 -lc" \
		CFLAGS="" \
		CPPFLAGS="" \
		makemd5
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/sasl $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsasl2.{a,so*} $(1)/usr/lib/
	ln -sf libsasl2.a $(1)/usr/lib/libsasl.a
	ln -sf libsasl2.so $(1)/usr/lib/libsasl.so
	$(INSTALL_DIR) $(1)/usr/lib/sasl2
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/lib*.{a,so*} $(1)/usr/lib/sasl2/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libsasl2.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libsasl2/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsasl2.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/sasl2
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libanonymous.so* $(1)/usr/lib/sasl2/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libplain.so* $(1)/usr/lib/sasl2/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libcrammd5.so* $(1)/usr/lib/sasl2/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libdigestmd5.so* $(1)/usr/lib/sasl2/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libscram.so* $(1)/usr/lib/sasl2/
endef

define Package/libsasl2-sasldb/install
	$(INSTALL_DIR) $(1)/usr/lib/sasl2
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sasl2/libsasldb.so* $(1)/usr/lib/sasl2/
endef

define Package/libsasl2-utils/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/{pluginviewer,sasldblistusers2,saslpasswd2} $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,libsasl2))
$(eval $(call BuildPackage,libsasl2-sasldb))
$(eval $(call BuildPackage,libsasl2-utils))
