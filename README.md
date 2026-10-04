# EXA Connect


## Overview

**EXA Connect** is an advanced, high-performance VPN client and networking utility designed to securely connect users to institutional or corporate networks. Built from the ground up for speed, reliability, and security, it supports robust tunneling and proxy capabilities, enabling seamless remote access and dynamic traffic routing.

## Key Functions

- **Multi-Protocol VPN Support**: Natively integrates complex network protocols (including EasyConnect and aTrust) for flexible, secure, and encrypted remote network connections.
- **System-Wide TUN Mode**: Creates a virtual network interface to intercept and route all local system TCP/UDP traffic securely through the VPN tunnel.
- **Built-in Proxy Services**: Acts as a local proxy server, providing SOCKS5, HTTP, and Shadowsocks proxy endpoints for application-level traffic forwarding.
- **Advanced DNS Management**: Features DNS hijacking, intelligent remote DNS resolution, DNS caching, and Fake IP functionalities to prevent DNS leaks and optimize connection speeds.
- **Port Forwarding**: Supports both TCP and UDP port forwarding, allowing specific internal institutional services to be exposed directly to the local machine.
- **Comprehensive Authentication**: Adapts to complex institutional login systems, supporting standard passwords, SMS verification, TOTP (Time-Based One-Time Password), CAS (Central Authentication Service), and Certificate-based (p12) authentication.

## System Architecture

EXA Connect is engineered in **Go (Golang)**, leveraging its powerful concurrency model (goroutines) to handle thousands of concurrent network streams with minimal memory overhead. The architecture is modular and highly decoupled.

### Core Modules:

1. **Network Stack (`/stack`)**
   - Implements a userspace TCP/IP networking stack (utilizing technologies like `gVisor` and custom TUN device drivers).
   - Responsible for intercepting raw IP packets from the virtual TUN interface, extracting the payload, and routing it through the encrypted VPN tunnel.

2. **Client Protocol Layer (`/client`)**
   - Contains the core logic for communicating with the remote VPN servers.
   - Manages TLS cryptographic negotiations, protocol handshakes, authentication flows, and session persistence (Keep-Alive logic).

3. **Service & Proxy Layer (`/service`)**
   - Exposes local services to the host machine.
   - Spins up lightweight local servers for SOCKS5 and HTTP proxies.
   - Handles the lifecycle of background tasks like DNS proxying and TCP/UDP port forwarding.

4. **Routing & Dialer (`/dial`, `/underlay`)**
   - Abstracts the underlying socket connections.
   - Intelligently decides whether a specific packet or connection should go through the VPN tunnel or bypass it (Direct Connection) based on customized IP and domain routing rules.

5. **DNS Resolver (`/resolve`)**
   - Intercepts local DNS queries.
   - Forwards specific institutional domain queries to the remote VPN DNS server while resolving public domains locally to ensure maximum browsing speed and privacy.

---
*Disclaimer: This software is provided as-is. The developer assumes no responsibility for network misuse.*
