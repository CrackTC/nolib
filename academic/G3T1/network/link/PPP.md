# PPP

- **DOES** provide error detection
- **DOES NOT** provide frame loss detection

## Frame Format

PPP frames are variants of [[HDLC]] frames.

| Flag | Address | Control | Protocol | Payload              | FCS | Flag |
| ---- | ------- | ------- | -------- | -------------------- | --- | ---- |
| 0x7E | 1B      | 1B      | 2B       | variable (1500B max) | 2B  | 0x7E |

- Flag: `0x7E` (01111110)
- Address: `0xFF` (peer-to-peer, address makes no sense)
- Control: usually `0x03`
- Protocol
  - `0x0021` for IP
  - `0x8021` for IPCP
  - `0xC021` for LCP
  - `0xC023` for PAP
