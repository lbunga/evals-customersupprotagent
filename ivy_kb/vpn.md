# VPN / Remote Access

The standard approved VPN client is **Palo Alto GlobalProtect**, available in the software portal. Third-party VPN clients are not supported.

## Setup
1. Install **GlobalProtect** from the software portal.
2. Enter the portal address **vpn.company.com**.
3. Authenticate with **SSO + MFA**.
4. Select the nearest gateway and connect.
If the network adapter does not appear, restart the machine.

## "License invalid" error on connect
Despite the wording, this is a connectivity/token issue, not a software license. Sign out and back in to refresh the GlobalProtect token. If it persists, reinstall the client from the software portal.
