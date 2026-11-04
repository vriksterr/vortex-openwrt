# vortex-openwrt

[Vortex](https://github.com/vriksterr/vortex) (updated fork of glorytun) package for OpenWrt.

Uses the target's `libsodium` (>= 1.0.4) rather than the bundled copy.

Add this line in your `feeds.conf.default`:

    src-git vortex https://github.com/vriksterr/glorytun-openwrt

Update and install your feeds:

    $ ./scripts/feeds update -a
    $ ./scripts/feeds install -a

Configure (Network -> VPN -> vortex) and build:

    $ make defconfig
    $ make menuconfig
    $ make package/vortex/compile

## Configuration (`/etc/config/vortex`)

    config mud main
        option enable '1'
        option key '<output of: vortex keygen>'
        option server '203.0.113.10'   # omit on the server side
        option port '5000'
        option dev 'vx0'
        # option bind '0.0.0.0'
        # option bind_port '0'

Migrating from the glorytun package: rename `/etc/config/glorytun` to
`/etc/config/vortex`. Do not assume Vortex interoperates with
upstream glorytun; run Vortex on both ends.

The package pins a vortex commit in `PKG_SOURCE_VERSION`; bump it to update.
