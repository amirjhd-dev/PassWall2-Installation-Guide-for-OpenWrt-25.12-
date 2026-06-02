# PassWall2 Installation Guide for OpenWrt 25.12+

Install PassWall2 and its required components on OpenWrt 25.12+ using APK package management.

---

## Requirements

Before starting, make sure:

* OpenWrt 25.12 or newer is installed.
* Internet access is available.
* You have SSH access as root.
* Enough free storage space is available.

---

# Step 1: Update Package Index

```bash
apk update
```
# Step 2 : Install wget-ssl and ca-bundle
```bash
apk add wget-ssl ca-bundle
```

# Step 3: Install Nano

```bash
apk add nano
```

---

# Step 4: Install Unzip

```bash
apk add unzip
```

---

# Step 5: Install Curl

```bash
apk add curl
```

---

# Step 6: Install DNSMasq Full

```bash
apk add dnsmasq-full
```

---

# Step 7: Install TProxy Module

```bash
apk add kmod-nft-tproxy
```

Required for Transparent Proxy (TProxy) mode.

---

# Step 8: Install Socket Module

```bash
apk add kmod-nft-socket
```

Required by nftables transparent proxy rules.

---

# Step 9: Find Your Device Information

Display your OpenWrt platform information:

```bash
ubus call system board
```

or

```bash
cat /etc/openwrt_release
```

Example output:

```text
OPENWRT_RELEASE="25.12.4"
DISTRIB_TARGET="ipq40xx/chromium"
DISTRIB_ARCH="arm_cortex-a7_neon-vfpv4"
```

Use these values to replace:

| Placeholder         | Description            |
| ------------------- | ---------------------- |
| `<OPENWRT_VERSION>` | OpenWrt version        |
| `<TARGET>`          | Device target          |
| `<SUBTARGET>`       | Device subtarget       |
| `<ARCH>`            | CPU architecture       |
| `<KERNEL_VERSION>`  | Kernel package version |

---

# Step 10: Configure APK Repositories

Open repository configuration:

```bash
nano /etc/apk/repositories
```

Add official OpenWrt repositories:

```text
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/targets/<TARGET>/<SUBTARGET>/packages/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/base/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/targets/<TARGET>/<SUBTARGET>/kmods/<KERNEL_VERSION>/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/luci/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/packages/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/routing/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/telephony/packages.adb
https://downloads.openwrt.org/releases/<OPENWRT_VERSION>/packages/<ARCH>/video/packages.adb
```

Add PassWall repositories matching your architecture:

```text
https://master.dl.sourceforge.net/project/openwrt-passwall-build/releases/packages-25.12/<ARCH>/passwall_luci/packages.adb
https://master.dl.sourceforge.net/project/openwrt-passwall-build/releases/packages-25.12/<ARCH>/passwall_packages/packages.adb
https://master.dl.sourceforge.net/project/openwrt-passwall-build/releases/packages-25.12/<ARCH>/passwall2/packages.adb
```

Save and exit Nano:

```text
CTRL + O
ENTER
CTRL + X
```

---

# Step 11: Import Repository Key

```bash
wget -O /etc/apk/keys/apk.pub https://downloads.sourceforge.net/project/openwrt-passwall-build/apk.pub
```

---

# Step 12: Refresh Repository Index

```bash
apk update --allow-untrusted
```

---

# Step 13: Install PassWall2

```bash
apk add luci-app-passwall2
```

---

# Step 14: Install Proxy Cores

```bash
apk add xray-core sing-box hysteria haproxy microsocks naiveproxy ip-full
```

---

# Installed Components

| Package    | Description                                     |
| ---------- | ----------------------------------------------- |
| xray-core  | Supports VLESS, VMess, Trojan, Reality and XTLS |
| sing-box   | Modern multi-protocol proxy platform            |
| hysteria   | High-performance UDP-based protocol             |
| haproxy    | Load balancer and reverse proxy                 |
| microsocks | Lightweight SOCKS5 server                       |
| naiveproxy | Chromium-based proxy protocol                   |
| ip-full    | Advanced networking utilities                   |

---

# Verification

Verify PassWall2 installation:

```bash
apk info | grep passwall
```

Verify installed cores:

```bash
which xray
which sing-box
which hysteria
which naive
```

Verify TProxy modules:

```bash
lsmod | grep tproxy
```

---

# Access PassWall2

Open LuCI and navigate to:

```text
Services → PassWall2
```

You can now:

* Import subscriptions
* Configure VLESS
* Configure VMess
* Configure Trojan
* Configure Reality
* Configure Sing-Box
* Configure Hysteria
* Configure NaiveProxy
* Configure SOCKS5
* Configure DNS Routing
* Configure Load Balancing

---

# Troubleshooting

## Repository Update Failed

Check:

* Internet connectivity
* Repository URLs
* OpenWrt version
* Architecture selection

## Missing Proxy Core

Install manually:

```bash
apk add xray-core
apk add sing-box
```

## TProxy Not Working

Verify required modules:

```bash
lsmod | grep nft
```

Required modules:

```text
nft_tproxy
nft_socket
```

---

# Notes

* OpenWrt 25.12+ uses APK instead of OPKG.
* Always use repositories matching your architecture.
* DNSMasq Full is recommended for advanced DNS functionality.
* TProxy mode requires both `kmod-nft-tproxy` and `kmod-nft-socket`.
* Additional proxy cores can be installed later if needed.

---

# License

This guide is provided for educational purposes. All package copyrights belong to their respective upstream projects.
