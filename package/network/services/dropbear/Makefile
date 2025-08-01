#
# Copyright (C) 2006-2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dropbear
PKG_VERSION:=2025.88
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:= \
	https://matt.ucc.asn.au/dropbear/releases/ \
	https://dropbear.nl/mirror/releases/
PKG_HASH:=783f50ea27b17c16da89578fafdb6decfa44bb8f6590e5698a4e4d3672dc53d4

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE libtomcrypt/LICENSE libtommath/LICENSE
PKG_CPE_ID:=cpe:/a:dropbear_ssh_project:dropbear_ssh

PKG_BUILD_PARALLEL:=1
PKG_ASLR_PIE_REGULAR:=1
PKG_BUILD_FLAGS:=no-mips16 gc-sections lto
PKG_FIXUP:=autoreconf
PKG_FLAGS:=nonshared

PKG_CONFIG_DEPENDS:= \
	CONFIG_DROPBEAR_STATIC_BUILD CONFIG_DROPBEAR_LEGACY_COMPAT CONFIG_DROPBEAR_SMALL_CODE CONFIG_DROPBEAR_DEBUG_TRACE CONFIG_DROPBEAR_LOG_COMMANDS CONFIG_DROPBEAR_INETD_MODE \
	CONFIG_DROPBEAR_DO_HOST_LOOKUP CONFIG_DROPBEAR_SVR_PUBKEY_OPTIONS CONFIG_DROPBEAR_LASTLOG CONFIG_DROPBEAR_LASTLOG_PATH CONFIG_DROPBEAR_WTMP CONFIG_DROPBEAR_WTMP_PATH CONFIG_DROPBEAR_UTMP CONFIG_DROPBEAR_UTMP_PATH CONFIG_DROPBEAR_PUTUTLINE CONFIG_DROPBEAR_LOGINFUNC \
	CONFIG_DROPBEAR_REEXEC CONFIG_DROPBEAR_ZLIB CONFIG_DROPBEAR_DELAY_HOSTKEY CONFIG_DROPBEAR_SVR_AGENTFWD CONFIG_DROPBEAR_SVR_REMOTETCPFWD CONFIG_DROPBEAR_SVR_LOCALTCPFWD CONFIG_DROPBEAR_SVR_LOCALSTREAMFWD CONFIG_DROPBEAR_X11FWD CONFIG_DROPBEAR_SCP CONFIG_DROPBEAR_SFTPSERVER \
	CONFIG_DROPBEAR_DBCLIENT CONFIG_DROPBEAR_USER_ALGO_LIST CONFIG_DROPBEAR_USE_SSH_CONFIG CONFIG_DROPBEAR_CLI_IMMEDIATE_AUTH CONFIG_DROPBEAR_USE_PASSWORD_ENV CONFIG_DROPBEAR_CLI_ASKPASS_HELPER CONFIG_DROPBEAR_CLI_AGENTFWD CONFIG_DROPBEAR_CLI_LOCALTCPFWD CONFIG_DROPBEAR_CLI_REMOTETCPFWD CONFIG_DROPBEAR_CLI_PROXYCMD CONFIG_DROPBEAR_CLI_NETCAT CONFIG_DROPBEAR_CLI_MULTIHOP \
	CONFIG_DROPBEAR_KEX_REKEY_TIMEOUT CONFIG_DROPBEAR_KEX_REKEY_DATA CONFIG_DROPBEAR_AUTH_TIMEOUT CONFIG_DROPBEAR_MAX_AUTH_TRIES CONFIG_DROPBEAR_UNAUTH_CLOSE_DELAY CONFIG_DROPBEAR_MAX_UNAUTH_PER_IP CONFIG_DROPBEAR_MAX_UNAUTH_CLIENTS CONFIG_DROPBEAR_DEFAULT_RECV_WINDOW CONFIG_DROPBEAR_DEFAULT_KEEPALIVE CONFIG_DROPBEAR_DEFAULT_KEEPALIVE_LIMIT CONFIG_DROPBEAR_DEFAULT_IDLE_TIMEOUT \
	CONFIG_DROPBEAR_3DES CONFIG_DROPBEAR_AES128 CONFIG_DROPBEAR_AES256 CONFIG_DROPBEAR_CHACHA20POLY1305 \
	CONFIG_DROPBEAR_ENABLE_CTR_MODE CONFIG_DROPBEAR_ENABLE_CBC_MODE CONFIG_DROPBEAR_ENABLE_GCM_MODE \
	CONFIG_DROPBEAR_SHA1_96_HMAC CONFIG_DROPBEAR_SHA1_HMAC CONFIG_DROPBEAR_SHA2_256_HMAC CONFIG_DROPBEAR_SHA2_512_HMAC \
	CONFIG_DROPBEAR_DSS CONFIG_DROPBEAR_RSA_SHA1 CONFIG_DROPBEAR_RSA CONFIG_DROPBEAR_DEFAULT_RSA_SIZE CONFIG_DROPBEAR_ECDSA CONFIG_DROPBEAR_ED25519 \
	CONFIG_DROPBEAR_DH_GROUP1 CONFIG_DROPBEAR_DH_GROUP1_CLIENTONLY CONFIG_DROPBEAR_DH_GROUP14_SHA1 CONFIG_DROPBEAR_DH_GROUP14_SHA256 CONFIG_DROPBEAR_DH_GROUP16 CONFIG_DROPBEAR_ECDH CONFIG_DROPBEAR_CURVE25519 CONFIG_DROPBEAR_MLKEM768 CONFIG_DROPBEAR_SNTRUP761 \
	CONFIG_DROPBEAR_ECC_256 CONFIG_DROPBEAR_ECC_384 CONFIG_DROPBEAR_ECC_521 \
	CONFIG_DROPBEAR_SK_KEYS CONFIG_DROPBEAR_SK_ECDSA CONFIG_DROPBEAR_SK_ED25519 \


include $(INCLUDE_DIR)/package.mk

ifneq ($(DUMP),1)
  STAMP_CONFIGURED:=$(strip $(STAMP_CONFIGURED))_$(shell echo $(CONFIG_TARGET_INIT_PATH) | $(MKHASH) md5)
endif

define Package/dropbear/Default
  URL:=https://matt.ucc.asn.au/dropbear/
endef

define Package/dropbear/config
	source "$(SOURCE)/Config.in"
endef

define Package/dropbear
  $(call Package/dropbear/Default)
  SECTION:=net
  CATEGORY:=Base system
  TITLE:=Small SSH2 client/server
  MENU:=1
  DEPENDS:= +USE_GLIBC:libcrypt-compat +DROPBEAR_ZLIB:zlib
  ALTERNATIVES:=100:/usr/bin/ssh-keygen:/usr/sbin/dropbear
  $(if $(CONFIG_DROPBEAR_SCP),ALTERNATIVES+= \
	  100:/usr/bin/scp:/usr/sbin/dropbear,)
  $(if $(CONFIG_DROPBEAR_DBCLIENT),ALTERNATIVES+= \
	  100:/usr/bin/ssh:/usr/sbin/dropbear,)

endef

define Package/dropbear/description
 A small SSH2 server/client designed for small memory environments.
endef

define Package/dropbear/conffiles
/etc/config/dropbear
/etc/dropbear/authorized_keys
/etc/dropbear/dropbear_ecdsa_host_key
/etc/dropbear/dropbear_ed25519_host_key
/etc/dropbear/dropbear_rsa_host_key
endef

define Package/dropbearconvert
  $(call Package/dropbear/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Utility for converting SSH keys
  DEPENDS:= +DROPBEAR_ZLIB:zlib
endef

CONFIGURE_ARGS += \
	--disable-pam \
	--disable-pututxline \
	--disable-utmpx \
	--disable-wtmpx \
	$(if $(CONFIG_DROPBEAR_LASTLOG),,--disable-lastlog) \
	$(if $(CONFIG_DROPBEAR_LOGINFUNC),,--disable-loginfunc) \
	$(if $(CONFIG_DROPBEAR_PUTUTLINE),,--disable-pututline) \
	$(if $(CONFIG_DROPBEAR_STATIC_BUILD),--enable-static,--disable-static) \
	$(if $(CONFIG_DROPBEAR_UTMP),,--disable-utmp) \
	$(if $(CONFIG_DROPBEAR_WTMP),,--disable-wtmp) \
	$(if $(CONFIG_DROPBEAR_ZLIB),,--disable-zlib) \
	--enable-bundled-libtom

##############################################################################
#
#   option,value - add option to localoptions.h
# !!option,value - replace option in src/sysoptions.h
#
##############################################################################

# adjust allowed shell list (if getusershell(3) is missing):
# - COMPAT_USER_SHELLS
# remove version number from protocol ident:
# - IDENT_VERSION_PART
# disable unused functionality:
# - DO_MOTD

DB_OPT_COMMON = \
	IDENT_VERSION_PART,"" \
	COMPAT_USER_SHELLS,"/bin/ash","/bin/sh" \
	DEFAULT_PATH,"$(TARGET_INIT_PATH)" \
	DEFAULT_ROOT_PATH,"$(TARGET_INIT_PATH)" \
	DO_MOTD,0 \
	AUTH_TIMEOUT,$(CONFIG_DROPBEAR_AUTH_TIMEOUT) \
	DEBUG_TRACE,$(CONFIG_DROPBEAR_DEBUG_TRACE) \
	DEFAULT_IDLE_TIMEOUT,$(CONFIG_DROPBEAR_DEFAULT_IDLE_TIMEOUT) \
	DEFAULT_KEEPALIVE_LIMIT,$(CONFIG_DROPBEAR_DEFAULT_KEEPALIVE_LIMIT) \
	DEFAULT_KEEPALIVE,$(CONFIG_DROPBEAR_DEFAULT_KEEPALIVE) \
	DEFAULT_RECV_WINDOW,$(CONFIG_DROPBEAR_DEFAULT_RECV_WINDOW) \
	KEX_REKEY_DATA,$(CONFIG_DROPBEAR_KEX_REKEY_DATA) \
	KEX_REKEY_TIMEOUT,$(CONFIG_DROPBEAR_KEX_REKEY_TIMEOUT) \
	MAX_AUTH_TRIES,$(CONFIG_DROPBEAR_MAX_AUTH_TRIES) \
	MAX_UNAUTH_CLIENTS,$(CONFIG_DROPBEAR_MAX_UNAUTH_CLIENTS) \
	MAX_UNAUTH_PER_IP,$(CONFIG_DROPBEAR_MAX_UNAUTH_PER_IP) \
	UNAUTH_CLOSE_DELAY,$(CONFIG_DROPBEAR_UNAUTH_CLOSE_DELAY) \
	$(if $(CONFIG_DROPBEAR_RSA),DROPBEAR_DEFAULT_RSA_SIZE$(comma)$(CONFIG_DROPBEAR_DEFAULT_RSA_SIZE)) \
	$(if $(CONFIG_DROPBEAR_LASTLOG),LASTLOG_FILE$(comma)$(CONFIG_DROPBEAR_LASTLOG_FILE)) \
	$(if $(CONFIG_DROPBEAR_UTMP),UTMP_FILE$(comma)$(CONFIG_DROPBEAR_UTMP_FILE)) \
	$(if $(CONFIG_DROPBEAR_WTMP),WTMP_FILE$(comma)$(CONFIG_DROPBEAR_WTMP_FILE)) \


##############################################################################
#
#   option,config,enabled,disabled = add option to localoptions.h
# !!option,config,enabled,disabled = replace option in src/sysoptions.h
#
#   option := (config) ? enabled : disabled
#
##############################################################################

DB_OPT_CONFIG = \
	!!DROPBEAR_CLI_MULTIHOP,CONFIG_DROPBEAR_CLI_MULTIHOP,1,0 \
	!!DROPBEAR_ECC_256,CONFIG_DROPBEAR_ECC_256,1,0 \
	!!DROPBEAR_ECC_384,CONFIG_DROPBEAR_ECC_384,1,0 \
	!!DROPBEAR_ECC_521,CONFIG_DROPBEAR_ECC_521,1,0 \
	DO_HOST_LOOKUP,CONFIG_DROPBEAR_DO_HOST_LOOKUP,1,0 \
	DROPBEAR_3DES,CONFIG_DROPBEAR_3DES,1,0 \
	DROPBEAR_AES128,CONFIG_DROPBEAR_AES128,1,0 \
	DROPBEAR_AES256,CONFIG_DROPBEAR_AES256,1,0 \
	DROPBEAR_CHACHA20POLY1305,CONFIG_DROPBEAR_CHACHA20POLY1305,1,0 \
	DROPBEAR_CLI_AGENTFWD,CONFIG_DROPBEAR_CLI_AGENTFWD,1,0 \
	DROPBEAR_CLI_ASKPASS_HELPER,CONFIG_DROPBEAR_CLI_ASKPASS_HELPER,1,0 \
	DROPBEAR_CLI_IMMEDIATE_AUTH,CONFIG_DROPBEAR_CLI_IMMEDIATE_AUTH,1,0 \
	DROPBEAR_CLI_LOCALTCPFWD,CONFIG_DROPBEAR_CLI_LOCALTCPFWD,1,0 \
	DROPBEAR_CLI_NETCAT,CONFIG_DROPBEAR_CLI_NETCAT,1,0 \
	DROPBEAR_CLI_PROXYCMD,CONFIG_DROPBEAR_CLI_PROXYCMD,1,0 \
	DROPBEAR_CLI_REMOTETCPFWD,CONFIG_DROPBEAR_CLI_REMOTETCPFWD,1,0 \
	DROPBEAR_CURVE25519,CONFIG_DROPBEAR_CURVE25519,1,0 \
	DROPBEAR_DELAY_HOSTKEY,CONFIG_DROPBEAR_DELAY_HOSTKEY,1,0 \
	DROPBEAR_DH_GROUP1,CONFIG_DROPBEAR_DH_GROUP1,1,0 \
	DROPBEAR_DH_GROUP1_CLIENTONLY,CONFIG_DROPBEAR_DH_GROUP1_CLIENTONLY,1,0 \
	DROPBEAR_DH_GROUP14_SHA1,CONFIG_DROPBEAR_DH_GROUP14_SHA1,1,0 \
	DROPBEAR_DH_GROUP14_SHA256,CONFIG_DROPBEAR_DH_GROUP14_SHA256,1,0 \
	DROPBEAR_DH_GROUP16,CONFIG_DROPBEAR_DH_GROUP16,1,0 \
	DROPBEAR_DSS,CONFIG_DROPBEAR_DSS,1,0 \
	DROPBEAR_ECDH,CONFIG_DROPBEAR_ECDH,1,0 \
	DROPBEAR_ECDSA,CONFIG_DROPBEAR_ECDSA,1,0 \
	DROPBEAR_ED25519,CONFIG_DROPBEAR_ED25519,1,0 \
	DROPBEAR_ENABLE_CBC_MODE,CONFIG_DROPBEAR_ENABLE_CBC_MODE,1,0 \
	DROPBEAR_ENABLE_CTR_MODE,CONFIG_DROPBEAR_ENABLE_CTR_MODE,1,0 \
	DROPBEAR_ENABLE_GCM_MODE,CONFIG_DROPBEAR_ENABLE_GCM_MODE,1,0 \
	DROPBEAR_MLKEM768,CONFIG_DROPBEAR_MLKEM768,1,0 \
	DROPBEAR_REEXEC,CONFIG_DROPBEAR_REEXEC,1,0 \
	DROPBEAR_RSA_SHA1,CONFIG_DROPBEAR_RSA_SHA1,1,0 \
	DROPBEAR_RSA,CONFIG_DROPBEAR_RSA,1,0 \
	DROPBEAR_SFTPSERVER,CONFIG_DROPBEAR_SFTPSERVER,1,0 \
	DROPBEAR_SHA1_96_HMAC,CONFIG_DROPBEAR_SHA1_96_HMAC,1,0 \
	DROPBEAR_SHA1_HMAC,CONFIG_DROPBEAR_SHA1_HMAC,1,0 \
	DROPBEAR_SHA2_256_HMAC,CONFIG_DROPBEAR_SHA2_256_HMAC,1,0 \
	DROPBEAR_SHA2_512_HMAC,CONFIG_DROPBEAR_SHA2_512_HMAC,1,0 \
	DROPBEAR_SK_ECDSA,CONFIG_DROPBEAR_SK_ECDSA,1,0 \
	DROPBEAR_SK_ED25519,CONFIG_DROPBEAR_SK_ED25519,1,0 \
	DROPBEAR_SK_KEYS,CONFIG_DROPBEAR_SK_KEYS,1,0 \
	DROPBEAR_SMALL_CODE,CONFIG_DROPBEAR_SMALL_CODE,1,0 \
	DROPBEAR_SNTRUP761,CONFIG_DROPBEAR_SNTRUP761,1,0 \
	DROPBEAR_SVR_AGENTFWD,CONFIG_DROPBEAR_SVR_AGENTFWD,1,0 \
	DROPBEAR_SVR_LOCALSTREAMFWD,CONFIG_DROPBEAR_SVR_LOCALSTREAMFWD,1,0 \
	DROPBEAR_SVR_LOCALTCPFWD,CONFIG_DROPBEAR_SVR_LOCALTCPFWD,1,0 \
	DROPBEAR_SVR_PUBKEY_OPTIONS,CONFIG_DROPBEAR_SVR_PUBKEY_OPTIONS,1,0 \
	DROPBEAR_SVR_REMOTETCPFWD,CONFIG_DROPBEAR_SVR_REMOTETCPFWD,1,0 \
	DROPBEAR_USE_PASSWORD_ENV,CONFIG_DROPBEAR_USE_PASSWORD_ENV,1,0 \
	DROPBEAR_USE_SSH_CONFIG,CONFIG_DROPBEAR_USE_SSH_CONFIG,1,0 \
	DROPBEAR_USER_ALGO_LIST,CONFIG_DROPBEAR_USER_ALGO_LIST,1,0 \
	DROPBEAR_X11FWD,CONFIG_DROPBEAR_X11FWD,1,0 \
	INETD_MODE,CONFIG_DROPBEAR_INETD_MODE,1,0 \
	LOG_COMMANDS,CONFIG_DROPBEAR_LOG_COMMANDS,1,0 \


TARGET_CFLAGS += -DARGTYPE=3

xsedx:=$(shell printf '\027')

db_opt_add     =echo '\#define $(1) $(2)' >> $(PKG_BUILD_DIR)/localoptions.h
db_opt_replace =$(ESED) '/^\#define $(1) .*$$$$/{h;:a;$$$$!n;/^\#.+$$$$/bb;/^$$$$/bb;H;ba;:b;x;s$(xsedx)^.+$$$$$(xsedx)\#define $(1) $(2)$(xsedx)p;x};p' -n $(PKG_BUILD_DIR)/src/sysoptions.h

define Build/Configure/dropbear_headers
	$(strip $(foreach s,$(DB_OPT_COMMON), \
	  $(if $(filter !!%,$(word 1,$(subst $(comma),$(space),$(s)))), \
	    $(call db_opt_replace,$(patsubst !!%,%,$(word 1,$(subst $(comma),$(space),$(s)))),$(subst $(space),$(comma),$(wordlist 2,$(words $(subst $(comma),$(space),$(s))),$(subst $(comma),$(space),$(s))))), \
	    $(call db_opt_add,$(word 1,$(subst $(comma),$(space),$(s))),$(subst $(space),$(comma),$(wordlist 2,$(words $(subst $(comma),$(space),$(s))),$(subst $(comma),$(space),$(s))))) \
	  ) ; \
	))

	$(strip $(foreach s,$(DB_OPT_CONFIG), \
	  $(if $(filter !!%,$(word 1,$(subst $(comma),$(space),$(s)))), \
	    $(call db_opt_replace,$(patsubst !!%,%,$(word 1,$(subst $(comma),$(space),$(s)))),$(if $($(word 2,$(subst $(comma),$(space),$(s)))),$(word 3,$(subst $(comma),$(space),$(s))),$(word 4,$(subst $(comma),$(space),$(s))))), \
	    $(call db_opt_add,$(word 1,$(subst $(comma),$(space),$(s))),$(if $($(word 2,$(subst $(comma),$(space),$(s)))),$(word 3,$(subst $(comma),$(space),$(s))),$(word 4,$(subst $(comma),$(space),$(s))))) \
	  ) ; \
	))
endef

define Build/Configure
	rm -f $(PKG_BUILD_DIR)/localoptions.h
	$(Build/Configure/Default)

	: > $(PKG_BUILD_DIR)/localoptions.h
	$(Build/Configure/dropbear_headers)

	# Enforce rebuild
	rm -rf $(PKG_BUILD_DIR)/obj
	+$(MAKE) -C $(PKG_BUILD_DIR)/libtomcrypt clean
	+$(MAKE) -C $(PKG_BUILD_DIR)/libtommath clean
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		IGNORE_SPEED=1 \
		PROGRAMS="dropbear $(if $(CONFIG_DROPBEAR_DBCLIENT),dbclient,) dropbearkey $(if $(CONFIG_DROPBEAR_SCP),scp,)" \
		MULTI=1 SCPPROGRESS=1
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		IGNORE_SPEED=1 \
		PROGRAMS="dropbearconvert"
endef

define Package/dropbear/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dropbearmulti $(1)/usr/sbin/dropbear
	$(INSTALL_DIR) $(1)/usr/bin
	$(if $(CONFIG_DROPBEAR_DBCLIENT),$(LN) ../sbin/dropbear $(1)/usr/bin/dbclient,)
	$(LN) ../sbin/dropbear $(1)/usr/bin/dropbearkey
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/dropbear.config $(1)/etc/config/dropbear
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dropbear.init $(1)/etc/init.d/dropbear
	$(INSTALL_DIR) $(1)/etc/dropbear
	$(INSTALL_DIR) $(1)/lib/preinit
	$(INSTALL_DATA) ./files/dropbear.failsafe $(1)/lib/preinit/99_10_failsafe_dropbear
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DATA) ./files/dropbear.defaults $(1)/etc/uci-defaults/50-dropbear
	$(foreach f,$(filter /etc/dropbear/%,$(Package/dropbear/conffiles)),$(if $(wildcard $(TOPDIR)/files/$(f)),chmod 0600 $(TOPDIR)/files/$(f) || :; ))
endef

define Package/dropbearconvert/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dropbearconvert $(1)/usr/bin/dropbearconvert
endef

$(eval $(call BuildPackage,dropbear))
$(eval $(call BuildPackage,dropbearconvert))
