# WLAN

## How to handle collisions when channel is shared?

Not like Ethernet, collision detection is difficult in wireless networks. The
signal sent by a station may not reach all other stations, so it is hard to
detect collision. Instead, wireless networks use collision avoidance.

### IFS (Interframe Space)

- SIFS (Short IFS)
  - Used for ACK, CTS, polling response
  - Shortest IFS
- PIFS (PCF IFS)
  - Used for PCF (Point Coordination Function)
  - Longer than SIFS (SIFS + 1 slot time)
- DIFS (DCF IFS)
  - Used for DCF (Distributed Coordination Function)
  - Longer than PIFS (PIFS + 1 slot time)

### CSMA/CA (Collision Avoidance)

- Check if the channel is idle for IFS before sending
- If channel is idle, send the frame
- If channel is busy
  - Wait for current transmission to finish
  - Wait for IFS
  - Wait for a random time before retrying, backoff time is doubled each time
