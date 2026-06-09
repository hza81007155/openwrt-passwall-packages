# SPDX-License-Identifier: GPL-2.0-or-later
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=tuic-client
PKG_VERSION:=1.8.6
PKG_RELEASE:=1

PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

include $(INCLUDE_DIR)/package.mk

ifeq ($(ARCH),aarch64)
	TUIC_ARCH:=tuic-client-aarch64-linux-musl
	PKG_HASH:=c2125374fca0765583724707dbf60c07cf0d4f880cc9b7d5a6fb4279c4e66914
else ifeq ($(ARCH),arm)
# Referred to golang/golang-values.mk
	ARM_CPU_FEATURES:=$(word 2,$(subst +,$(space),$(call qstrip,$(CONFIG_CPU_TYPE))))
	ifeq ($(ARM_CPU_FEATURES),)
		TUIC_ARCH:=tuic-client-armv7-linux-musl
		PKG_HASH:=8258b66130b06bb5a61bdb7114fd170cecd70818f6b3f5344f7d0b80a172bb64
	else
		TUIC_ARCH:=tuic-client-armv7-linux-muslhf
		PKG_HASH:=41db7376fb3090742edb2f40a6a8ade5e09d846dc8963f7d93352b75526af5ef
	endif
else ifeq ($(ARCH),i386)
	TUIC_ARCH:=tuic-client-i686-linux-musl
	PKG_HASH:=8eaf54edb88fc77b0ca129cf0414fb283cc8e233280194e37e56a5c205289804
else ifeq ($(ARCH),x86_64)
	TUIC_ARCH:=tuic-client-x86_64-linux-musl
	PKG_HASH:=e51931c08de29201fc6e7992b8fda45eafed94ee4a6d0d167e3f18faafbfb7d4
# Set the default value to make OpenWrt Package Checker happy
else
  PKG_SOURCE:=dummy
  PKG_HASH:=dummy
endif

define Download/tuic-client
	URL:=https://github.com/Itsusinn/tuic/releases/download/v$(PKG_VERSION)/
	URL_FILE:=$(TUIC_ARCH)
	FILE:=$(TUIC_ARCH)-$(PKG_VERSION)
	HASH:=$(PKG_HASH)
endef

$(eval $(call Download,tuic-client))

define Package/tuic-client
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=Delicately-TUICed 0-RTT proxy protocol
	URL:=https://github.com/Itsusinn/tuic
	DEPENDS:=@USE_MUSL @(aarch64||arm_v7||i386||x86_64) @!(TARGET_x86_geode||TARGET_x86_legacy)
endef

define Build/Compile
endef

define Package/tuic-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(DL_DIR)/$(TUIC_ARCH)-$(PKG_VERSION) $(1)/usr/bin/tuic-client
endef

$(eval $(call BuildPackage,tuic-client))
