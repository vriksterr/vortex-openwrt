include $(TOPDIR)/rules.mk

PKG_NAME:=vortex
PKG_VERSION:=2026.11.04
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/vriksterr/vortex.git
PKG_SOURCE_VERSION:=1509aac6d7f6854e729827c4bd4754a73f1f3b49
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=BSD-3-Clause
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/vortex
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	DEPENDS:=+kmod-tun +libsodium +libpthread
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

# Use the target's libsodium instead of the bundled copy.
define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) vortex \
		SODIUM=system \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS) -std=gnu99 -pthread" \
		CPPFLAGS="$(TARGET_CPPFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		LDLIBS="-lsodium -lpthread"
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
