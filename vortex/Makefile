include $(TOPDIR)/rules.mk

PKG_NAME:=vortex
PKG_VERSION:=2026.11.05
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/vriksterr/vortex.git
PKG_SOURCE_VERSION:=ba339b6
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=BSD-3-Clause
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/vortex
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	DEPENDS:=+kmod-tun
	TITLE:=Vortex multipath UDP tunnel
	URL:=https://github.com/vriksterr/vortex
endef

define Package/vortex/description
	Vortex is a small, simple and secure multipath UDP tunnel
	(a fork of glorytun with the vmux transport):
	- Fast and highly secure (AEGIS-256 / ChaCha20-Poly1305).
	- Multipath aggregation and active failover.
	- Traffic shaping and fixed, operator-set MTU.
	- Multi-client servers.
endef

define Package/vortex/conffiles
/etc/config/vortex
endef

# Builds the libsodium that ships in the vortex source and links it in
# statically, so the package needs no libsodium package on the router.
define Build/Compile
	cd $(PKG_BUILD_DIR) && sh ./sodium.sh $(TARGET_CROSS)
	$(MAKE) -C $(PKG_BUILD_DIR) vortex \
		X="$(TARGET_CROSS)" \
		CFLAGS="-std=gnu99 -O2 -Wall -fstack-protector-strong -pthread $(TARGET_CFLAGS)"
endef

define Package/vortex/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/vortex $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./init $(1)/etc/init.d/vortex
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./vortex.config $(1)/etc/config/vortex
endef

$(eval $(call BuildPackage,vortex))
