# Ethernet

Each station has a NIC (Network Interface Card) that has a unique 6-byte MAC address.

## How to handle collisions when channel is shared?

### CSMA (Carrier Sense Multiple Access)

- Check if the channel is idle before sending
- If channel is busy, wait for a random time before retrying
- Can still have collisions when two stations start sending at the same time

### CSMA/CD (Collision Detection)

- Detect collision by comparing the sent signal with the received signal
- If collision is detected, stop sending and wait for some time before retrying

#### Minimum Frame Size

- To ensure that the collision is detected before the frame is completely sent
- Should be 2 \* Propagation Delay \* Bandwidth

For 10 Mbps Ethernet, the minimum frame size is 512 bits (64 bytes), maximum
transmission distance is 5km. Every frame less than 64 bytes is corrupted by the
collision.

#### How long to wait before retrying? (Binary Exponential Backoff)

- After the k-th collision, wait for a random number of time units between 0 and 2^k - 1
- Each time unit equals to 2 \* Propagation Delay, so that retry time is aligned
- The maximum value of k is 10
- After 16th collision, give up and report error

## MAC Address

- 6 bytes, 48 bits
  - First 3 bytes: Organizationally Unique Identifier (OUI)
  - Last 3 bytes: NIC specific

- Multicast address: last bit of the first byte is `1`
- Broadcast address: all bits are `1`

## Ethernet MAC Frame Format

|        | Destination MAC | Source MAC | Type    | Data          | FCS     |
| ------ | --------------- | ---------- | ------- | ------------- | ------- |
| Length | 6 bytes         | 6 bytes    | 2 bytes | 46-1500 bytes | 4 bytes |

- Type: 2 bytes, indicates the type of upper layer protocol
  - > 0x0600: Ethernet II (DIX V2)
  - <= 0x0600: IEEE 802.3, indicates the length of the data field
- Data: 46-1500 bytes, padding is added if the data is less than 46 bytes
- FCS (Frame Check Sequence): 4 bytes, CRC-32

Some typical type values:
- 0x0800: IPv4
- 0x0806: ARP
- 0x8100: VLAN
- 0x86DD: IPv6

In physical layer, 7 bytes of preamble and 1 byte of start frame delimiter (SFD) are added before the frame.
