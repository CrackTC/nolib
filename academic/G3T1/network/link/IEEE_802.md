# IEEE 802 Standards

Data Link Layer divided into two sublayers:

- Logical Link Control (LLC, upper sublayer)
- Media Access Control (MAC, lower sublayer)

Serveral IEEE 802 standards:

- IEEE 802.1: General standard for LANs
- IEEE 802.2: Logical Link Control (LLC)
- MAC standards for specific medium:
  - IEEE 802.3: [[Ethernet]]
  - IEEE 802.4: Token Bus
  - IEEE 802.5: Token Ring
  - IEEE 802.11: [[WLAN|Wireless LAN]]
  - IEEE 802.15: Wireless PAN (Bluetooth, ZigBee, etc.)
  - IEEE 802.16: Broadband Wireless

## Service Access Points (SAPs)

Each station can have multiple SAPs, which are used to access services provided by the data link layer.

## Logical Link Control (LLC)

LLC exposes services to the network layer and has knowledge of SAPs.

Provides three types of services:
- Connectionless
  - Unacknowledged connectionless service (Suitable for wired networks)
  - Acknowledged connectionless service (Suitable for wireless networks)
- Connection-oriented (Suitable for applications communicate directly on top of the data link layer)

### LLC Frame Format

| Destination SAP | Source SAP | Control | Data |
| --------------- | ---------- | ------- | ---- |

#### Destination SAP

- First bit `0`: Individual address
- First bit `1`: Group address
- All `1`s: Broadcast address

#### Source SAP

- First bit `0`: Command frame
- First bit `1`: Response frame

#### Control

Like the control field in HDLC.

Three types of frames:
- Information frames
- Supervisory frames
- Unnumbered frames

but differs in some details.

- No SREJ (Selective Reject) frame
- No FCS (Frame Check Sequence), it is handled by the MAC sublayer

#### Data

## Media Access Control (MAC)

MAC is responsible for controlling access to the network medium and has knowledge of the physical addressing (MAC addresses).
