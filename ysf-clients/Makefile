#
# Copyright (C) 2008-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=YSF-clients
#PKG_REV:=HEAD
PKG_REV:=ec16283
PKG_VERSION:=BG5HHP-20181004
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/lazywalker/YSFClients
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_PROTO:=git

include $(INCLUDE_DIR)/package.mk

define Package/YSF-clients
  SECTION:=net
  CATEGORY:=Network
  TITLE:=YSFClients
  URL:=https://github.com/g4klx/YSFClients/
  DEPENDS:=+libpthread +libstdcpp
endef

define Package/YSF-clients/description
	The YSFClients package including YSFGateway,YSFParrot. 
	Build for OpenWrt, by BG5HHP
endef

TARGET_CFLAGS += -std=c++0x -DOPENWRT=1

TARGET_LDFLAGS += \
        -Wl,-rpath-link=$(STAGING_DIR)/usr/lib \

MAKE_FLAGS +=\
        CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
        LDFLAGS="$(TARGET_LDFLAGS)" \

define Package/YSF-clients/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/YSFGateway/YSFGateway $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/YSFParrot/YSFParrot $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/YSFReflector/YSFReflector $(1)/usr/sbin/
	$(CP) -a files/* $(1)/
endef

define Package/YSF-clients/conffiles
/etc/YSFGateway.ini
/etc/YSFReflector.ini
endef

define Package/YSF-clients/postinst
#!/bin/sh
if [ -e /etc/openwrt_release ]; then
	uci set mmdvm.@YSFgateway[0].enabled=1 && uci commit mmdvm || :
	[ -f /etc/init.d/YSFgateway ] && /etc/init.d/YSFgateway enable || :
fi
exit 0
endef

$(eval $(call BuildPackage,YSF-clients))
