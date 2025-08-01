#
# Copyright (C) 2006-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sudo
PKG_REALVERSION:=1.9.17p1
PKG_VERSION:=$(subst p,_p,$(PKG_REALVERSION))
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_REALVERSION).tar.gz
PKG_SOURCE_URL:=https://www.sudo.ws/dist
PKG_HASH:=ff607ea717072197738a78f778692cd6df9a7e3e404565f51de063ca27455d32
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_REALVERSION)

PKG_MAINTAINER:=Alexandru Ardelean <ardeleanalex@gmail.com>

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=doc/LICENSE
PKG_CPE_ID:=cpe:/a:sudo_project:sudo

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

include $(INCLUDE_DIR)/package.mk

define Package/sudo
  SECTION:=admin
  CATEGORY:=Administration
  TITLE:=Delegate authority to run commands
  URL:=https://www.sudo.ws/
  DEPENDS:= +USE_GLIBC:libcrypt-compat
endef

define Package/sudo/description
 Sudo (su "do") allows a system administrator to delegate authority to
 give certain users (or groups of users) the ability to run some (or
 all) commands as root or another user while providing an audit trail of
 the commands and their arguments.
endef

define Package/sudo/conffiles
/etc/sudoers
/etc/sudoers.d/
endef

CONFIGURE_ARGS += \
	--without-pam \
	--disable-pam-session \
	--with-editor=/bin/vi \
	--without-lecture \
	--disable-zlib \
	--disable-openssl \
	--with-rundir=/var/lib/sudo \
	--with-vardir=/var/lib/sudo

CONFIGURE_VARS += \
	sudo_cv_uid_t_len=10 \
	sudo_cv_func_unsetenv_void=no

define Package/sudo/install
	$(INSTALL_DIR) \
		$(1)/etc/{init.d,sudoers.d} \
		$(1)/usr/{bin,sbin} \
		$(1)/usr/lib/sudo

	$(CP) $(PKG_INSTALL_DIR)/usr/bin/sudo $(1)/usr/bin/
	chmod 4755 $(1)/usr/bin/sudo
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/visudo $(1)/usr/sbin/
	$(CP) $(PKG_INSTALL_DIR)/etc/sudoers $(1)/etc/
	chmod 0440 $(1)/etc/sudoers
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sudo/*.so* $(1)/usr/lib/sudo/
	$(INSTALL_BIN) ./files/sudo.init $(1)/etc/init.d/sudo
endef

define Package/sudo/postinst
#!/bin/sh

[ -n "$$IPKG_INSTROOT" ] || {
	/etc/init.d/sudo enable
	/etc/init.d/sudo start
}
endef

$(eval $(call BuildPackage,sudo))
