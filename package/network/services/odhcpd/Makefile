#
# Copyright (C) 2013-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=odhcpd
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=$(LEDE_GIT)/project/odhcpd.git
PKG_SOURCE_DATE:=2017-11-21
PKG_SOURCE_VERSION:=057342245c79fbec788ac9873a85de77d0c0a5b8
PKG_MIRROR_HASH:=20ca7c78a38555b125337538f33a1cf0e79f0062259d387f3ada57019086e8e7

PKG_MAINTAINER:=Hans Dedecker <dedeckeh@gmail.com>
PKG_LICENSE:=GPL-2.0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS += -DUBUS=1

define Package/odhcpd
  SECTION:=net
  CATEGORY:=Network
  TITLE:=OpenWrt DHCP/DHCPv6(-PD)/RA Server & Relay
  DEPENDS:=+libubox +libuci +libubus +libnl-tiny
endef

define Package/odhcpd/config
menu "Configuration"
	depends on PACKAGE_odhcpd

config PACKAGE_odhcpd_ext_cer_id
	int
	default 0
	prompt "CER-ID Extension ID (0 = disabled)"

config PACKAGE_odhcpd_dhcpv4_support
	bool
	default n
	prompt "Enable DHCPv4 server support"
endmenu
endef

ifneq ($(CONFIG_PACKAGE_odhcpd_ext_cer_id),0)
  CMAKE_OPTIONS += -DEXT_CER_ID=$(CONFIG_PACKAGE_odhcpd_ext_cer_id)
endif

ifeq ($(CONFIG_PACKAGE_odhcpd_dhcpv4_support),y)
  CMAKE_OPTIONS += -DDHCPV4_SUPPORT=1
endif


define Package/odhcpd/description
 odhcpd is a daemon for serving and relaying IP management protocols to
 configure clients and downstream routers. It tries to follow the RFC 6204
 requirements for IPv6 home routers.
 
 odhcpd provides server services for DHCP, RA, stateless and stateful DHCPv6,
 prefix delegation and can be used to relay RA, DHCPv6 and NDP between routed
 (non-bridged) interfaces in case no delegated prefixes are available.
endef

define Package/odhcpd/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/odhcpd $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/odhcpd-update $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/odhcpd.init $(1)/etc/init.d/odhcpd
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/odhcpd.defaults $(1)/etc/uci-defaults
endef

$(eval $(call BuildPackage,odhcpd))
