# ditana-network

This package provides the network configuration for Ditana GNU/Linux, including DNS, SSH, and NetworkManager settings.

## Overview

`ditana-network` is designed to establish a secure and privacy-focused network environment in Ditana. It configures NetworkManager, SSH, and systemd-resolved to ensure consistent and robust DNS resolution while enhancing security through modern protocols like DNSSEC and DNS-over-TLS.

## Package Components

1. **NetworkManager DNS Configuration** (`/etc/NetworkManager/conf.d/dns.conf`)  
   Configures NetworkManager to delegate DNS resolution to systemd-resolved, centralizing all DNS management under systemd.

2. **SSH Configuration** (`/etc/ssh/sshd_config.d/80-ditana.conf`)  
   Enables public key authentication by default, reinforcing secure remote access via SSH.

3. **systemd-resolved Configuration** (`/etc/systemd/resolved.conf.d/90-ditana.conf`)  
   Sets Cloudflare as the primary DNS provider and Quad9 as the fallback. It also enables DNSSEC validation and opportunistic DNS-over-TLS encryption for enhanced security.

## DNS Configuration Details

### Centralized DNS Management

Ditana GNU/Linux utilizes **systemd-resolved** to centralize DNS configuration and management, avoiding conflicts and inconsistencies that can arise from multiple DNS resolvers. The package explicitly avoids creating or linking a traditional `/etc/resolv.conf` file, instead relying on **systemd-resolved's internal mechanisms**. This setup ensures a streamlined approach to DNS handling and offers compatibility with most network-dependent applications.

However, it’s worth noting that while `/etc/resolv.conf` is not directly managed by this package, **systemd-resolved** dynamically maintains this file.

### Enhanced Security with DNSSEC and DNS-over-TLS

Ditana GNU/Linux employs two key DNS security technologies:

1. **DNSSEC (Domain Name System Security Extensions):**  
   DNSSEC adds a layer of security to DNS queries by authenticating the origin of DNS data. It prevents DNS spoofing and ensures that users connect to legitimate websites by validating the authenticity of DNS responses.

   **Benefits of DNSSEC:**
   - **Authentication:** Verifies that DNS records haven't been tampered with.
   - **Trust:** Enhances user confidence in online transactions by maintaining the integrity of DNS data.

2. **DNS-over-TLS:**  
   DNS-over-TLS encrypts DNS queries, protecting them from eavesdropping and interception. This technology ensures confidentiality for DNS requests, complementing the integrity checks provided by DNSSEC.

   **Benefits of DNS-over-TLS:**
   - **Privacy:** Shields DNS queries from third-party monitoring.
   - **Security:** Prevents tampering with DNS queries in transit.

### Quad9 and Cloudflare DNS Servers

By default, Ditana configures the following DNS servers:

- **Primary:** Quad9 DNS (IPv4: `9.9.9.9`, `149.112.112.112`, and IPv6 equivalents: `2620:fe::fe`, `2620:fe::9`)
- **Fallback:** Cloudflare DNS (IPv4: `1.1.1.1`, `1.0.0.1`, and IPv6 equivalents: `2606:4700:4700::1111`, `2606:4700:4700::1001`)

**Quad9** was chosen as the primary DNS service due to its outstanding security and privacy features:
- Automatic blocking of domain queries to known threats such as malware, phishing, and botnets
- No logging of user IP addresses
- Operated as a Swiss foundation with a clear focus on privacy
- Supported by over 25 threat intelligence providers for current threat information
- Global infrastructure with more than 230 resolver clusters in over 110 countries

**Cloudflare** serves as a reliable fallback DNS service and offers:
- Exceptionally fast response times
- Proven and stable infrastructure
- Broad global availability

Both DNS services fully support the security features configured in Ditana, such as DNSSEC and DNS-over-TLS, ensuring secure and private DNS resolution. Both services are also fully compliant with GDPR (General Data Protection Regulation) requirements.

### Why This Matters

With these configurations, Ditana users benefit from secure and private DNS resolution by default. These measures automatically protect against certain types of attacks like DNS spoofing, man-in-the-middle attacks, and unauthorized data collection. Users do not need to take additional steps to secure their DNS settings, as this package provides a robust and streamlined configuration.

## Installation

The `ditana-network` package is installed automatically as part of Ditana GNU/Linux and is not intended for manual installation. To update the package manually, use the following command:

```bash
sudo pacman -S ditana-network
```

## Package Dependencies

- `networkmanager`
- `inetutils`
- `systemd`

## Configuration Files

1. **NetworkManager DNS Configuration (`/etc/NetworkManager/conf.d/dns.conf`)**:
   ```ini
   [main]
   dns=systemd-resolved
   ```

2. **SSH Configuration (`/etc/ssh/sshd_config.d/80-ditana.conf`)**:
   ```ini
   # sshd_config defaults on Ditana GNU/Linux
   PubkeyAuthentication yes
   ```

3. **systemd-resolved Configuration (`/etc/systemd/resolved.conf.d/90-ditana.conf`)**:
   ```ini
   # Centralized DNS management under systemd-resolved
   [Resolve]
   DNS=
   FallbackDNS=
   DNS=1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
   FallbackDNS=9.9.9.9 149.112.112.112 2620:fe::fe 2620:fe::9
   DNSSEC=allow-downgrade
   DNSOverTLS=opportunistic
   Domains=~.
   ```

For more information about Ditana GNU/Linux, visit [https://ditana.org](https://ditana.org)
