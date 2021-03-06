#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# ARMv5 support is dropped in current versions:
# https://github.com/nodejs/node/issues/3061#issuecomment-143308732

include $(TOPDIR)/rules.mk

PKG_NAME:=node_legacy
PKG_VERSION:=v0.10.46
PKG_RELEASE:=1

PKG_SOURCE:=node-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://nodejs.org/dist/$(PKG_VERSION)

HOST_BUILD_DEPENDS:=python/host
PKG_BUILD_DEPENDS:=python/host
PKG_INSTALL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_DIR:=$(BUILD_DIR)/node-$(PKG_VERSION)
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/node-$(PKG_VERSION)

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>
PKG_LICENSE:=
PKG_BUILD_PARALLEL:=0

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/node_legacy
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Node.js
  TITLE:=Node.js is a platform built on Chrome's JavaScript runtime
  URL:=http://nodejs.org/
  DEPENDS:=+libstdcpp +libopenssl +libuv +ca-certificates @arm
endef

define Package/node_legacy/description
  Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses
  an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js'
   package ecosystem, npm, is the largest ecosystem of open source libraries in the world.
endef

CPU:=$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))

MAKE_VARS += \
	DESTCPU=$(CPU)

CONFIGURE_ARGS= \
	--dest-cpu=$(CPU) \
	--dest-os=linux \
	--without-snapshot \
	--shared-zlib \
	--shared-openssl \
	--prefix=/opt \
	--with-arm-float-abi=softfp \
	--with-mips-float-abi=soft

HOST_CONFIGURE_VARS:=
HOST_CONFIGURE_ARGS:= \
	--dest-os=linux \
	--without-snapshot \
	--prefix=$(STAGING_DIR)/host

HOST_CONFIGURE_CMD:=python ./configure

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include
	$(CP) $(PKG_INSTALL_DIR)/opt/include/* $(1)/opt/include
endef

define Package/node_legacy/install
	mkdir -p $(1)/opt/bin $(1)/opt/lib/node_modules/npm/{bin,lib,node_modules}
	$(CP) $(PKG_INSTALL_DIR)/opt/bin/{node,npm} $(1)/opt/bin
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/node_modules/npm/{package.json,LICENSE,cli.js} $(1)/opt/lib/node_modules/npm
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/node_modules/npm/bin/npm-cli.js $(1)/opt/lib/node_modules/npm/bin
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/node_modules/npm/lib/* $(1)/opt/lib/node_modules/npm/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/node_modules/npm/node_modules/* $(1)/opt/lib/node_modules/npm/node_modules
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,node_legacy))
