#
# Copyright (C) 2023 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=udebug
CMAKE_INSTALL:=1
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=$(PROJECT_GIT)/project/udebug.git
PKG_MIRROR_HASH:=f995d320fecaef3a149810b804c2428668552764a6828d323fef035f2211f2f4
PKG_SOURCE_DATE:=2023-12-05
PKG_SOURCE_VERSION:=a296f1bac4302c2102a0d2fd1e825c4dadddf0f4
PKG_ABI_VERSION:=$(call abi_version_str,$(PKG_SOURCE_DATE))

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libudebug
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=udebug client library
  DEPENDS:=+libubox +libubus
endef

define Package/udebugd
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=OpenWrt debug service
  DEPENDS:=+libudebug
endef

define Package/udebugd/conffiles
/etc/config/udebug
endef

define Package/ucode-mod-udebug
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=ucode udebug module
  DEPENDS:=+libucode +libudebug
endef

define Package/udebug-cli
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=OpenWrt debug service CLI
  DEPENDS:=+udebugd +ucode-mod-udebug
endef

define Package/libudebug/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
endef

define Package/ucode-mod-udebug/install
	$(INSTALL_DIR) $(1)/usr/lib/ucode
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ucode/*.so $(1)/usr/lib/ucode
endef

define Package/udebugd/install
	$(INSTALL_DIR) $(1)/usr/sbin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) ./files/udebug.config $(1)/etc/config/udebug
	$(INSTALL_BIN) ./files/udebug.init $(1)/etc/init.d/udebug
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/udebugd $(1)/usr/sbin
endef

define Package/udebug-cli/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/udebug-cli $(1)/usr/sbin/udebug
endef

$(eval $(call BuildPackage,libudebug))
$(eval $(call BuildPackage,udebugd))
$(eval $(call BuildPackage,ucode-mod-udebug))
$(eval $(call BuildPackage,udebug-cli))
